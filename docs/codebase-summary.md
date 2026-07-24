# Codebase Summary — maybe-finance/maybe

Repo: https://github.com/maybe-finance/maybe (Rails app, no longer actively maintained as of v0.6.0 — noted as a real risk, see below)
Purpose of this doc: orient a PM writing a spec for a **weekly money summary** feature (top spending insight, one contextual nudge, savings goal progress) against this codebase as a reference implementation.

---

## 1. PM-Level Tour

### What this product does, in one sentence
Maybe is a self-hostable (or Maybe-managed) personal finance app that connects bank/investment accounts via Plaid or CSV import and gives users budgeting, net worth, and transaction-categorization tools, plus an AI chat assistant over their financial data.

### How the codebase is organized

Standard Rails app — the folders that matter for product work:

| Folder | What it does |
|---|---|
| `app/models/` | All business logic lives here (this codebase's convention: "fat models, skinny controllers, no services layer"). This is where you'll find the actual domain rules — how a budget is calculated, how balances roll up, etc. |
| `app/controllers/` | Thin — mostly just orchestration and rendering. Good for understanding *what pages/actions exist*, not *how they work*. |
| `app/views/` + `app/components/` | UI. Uses ViewComponents for reusable pieces, plain ERB partials for one-off content. Hotwire (Turbo + Stimulus) — minimal custom JS. |
| `app/jobs/` | Background jobs (Sidekiq). Currently: syncing, imports, auto-categorization, AI chat responses, scheduled maintenance. **No digest/email-summary job exists today.** |
| `app/mailers/` | Only 4 mailers exist: password reset, invitation, email confirmation, and a base class. **No recurring/digest mailer exists today.** |
| `config/schedule.yml` | Cron-style recurring job definitions (market data import, sync cleanup, security health checks). This is where a "run every Monday" job would be registered. |
| `db/` | Migrations and schema — ground truth for what data actually exists. |
| `docs/` | Nearly empty in this repo (just hosting + one API doc) — don't expect written architecture docs beyond `CLAUDE.md`. |

### The 3 most important files I should know about as a PM

1. **`CLAUDE.md`** (repo root) — not code, but the single best PM-readable file in the repo. It states the team's actual engineering conventions in plain language: fat models/skinny controllers, no services layer, Hotwire-first frontend, minimize dependencies. Any spec you write should respect these constraints or explicitly call out that you're asking for an exception.
2. **`app/models/family.rb`** — the top of the domain model. Nearly everything (accounts, transactions, budgets, categories) hangs off `Family`, not `User`. This matters for product decisions: **budgets, categories, and merchants are shared at the family level, not per-user.** A "personalized" feature needs to decide whether personalization happens at the user or family level — the data model defaults to family.
3. **`app/models/income_statement.rb`** (plus `app/models/budget_category.rb`) — this is the closest existing thing to "insight generation" logic. It already computes `median_expense` / `avg_expense` per category, which is exactly the "your usual average" comparison a weekly insight needs. Read this before asking engineering to build comparison logic from scratch — some of it may already exist.

### Key data models and what they tell you about product decisions

- **`Family` → `Account` → `Entry`/`Transaction`**: the core chain. `Entry` is a polymorphic base (transactions, trades, valuations); `Transaction` is the one you care about.
- **`Category` / `Merchant`**: transactions can be categorized and merchant-matched, with auto-categorization and auto-merchant-detection jobs already built. This is good news for a "top spending category" insight — the categorization pipeline already exists.
- **`Budget` / `BudgetCategory`**: **this is a monthly spending plan, not a goal.** It tracks `budgeted_spending` vs `actual_spending` per category per month, with `avg_monthly_expense` / `median_monthly_expense` already computed. It is *not* a target-amount-with-a-deadline concept (e.g., "$1,000 Emergency Fund" or "Europe trip").
- **No "Goal" or "SavingsGoal" model exists anywhere in this codebase.** A direct search for "goal" across `app/models` and `app/controllers` turns up nothing relevant. This is the single biggest data-model gap relative to the Nudge feature.
- **`Assistant` / `Chat` / `Provider::Openai`**: there's a working LLM/AI chat layer already integrated (used for an in-app financial assistant chat). It's built for interactive chat, not batch/scheduled content generation, but it's evidence the team already has an LLM integration pattern to follow if the "contextual nudge" copy needs to be AI-generated rather than templated.
- **`MobileDevice`**: registers a device for OAuth/API token issuance for the mobile app. **It is not a push-notification token store** — there's no APNs/FCM integration visible anywhere in the codebase.

---

## 2. Mapping the Weekly Summary Feature to the Codebase

### Where would this feature live?

- **Backend logic**: a new PORO or model-adjacent class, following this codebase's own convention ("Models should answer questions about themselves" — no services layer). Likely something like `app/models/weekly_summary.rb`, initialized from a `Family` (or `User`, if personalization needs to be per-person — see open question below), similar in spirit to how `family.income_statement` and `family.balance_sheet` are already memoized objects on `Family`.
- **Delivery**: a new mailer (`app/mailers/weekly_summary_mailer.rb`) alongside a new job (`app/jobs/weekly_summary_job.rb`), registered in `config/schedule.yml` with a Monday cron entry — following the exact pattern already used for `import_market_data`.
- **In-app screen**: a new controller action (e.g., `WeeklySummariesController#show`), likely rendered near the existing dashboard (`root "pages#dashboard"`), using ViewComponents for the insight/nudge/goal cards per this codebase's component-vs-partial convention.

### What existing components would it touch or depend on?

- **`IncomeStatement`** and **`BudgetCategory`** — for the "top spending insight vs. your usual average" comparison; this logic substantially already exists.
- **`Category`** and **`Merchant`** — for labeling *what* the top insight/nudge is about, and for the auto-categorization pipeline that makes the data trustworthy in the first place.
- **`Family`** — as the root object everything else hangs off; also the multi-currency (`Money`) handling for any dollar amounts shown.
- **`ApplicationMailer`** and the mailer layout — for the email delivery piece.
- **`config/schedule.yml`** and the existing scheduled-job pattern — for the "every Monday" trigger.
- **Nothing existing to depend on for goals** — this is net-new (see below).

### Blast radius — what else could break if this ships with a bug?

- **Low risk to core financial data**: if the feature is read-only (computing and displaying insights, not writing back to transactions/budgets), it can't corrupt existing balances, transactions, or budgets. That's a meaningful scoping decision to make explicit in the ticket.
- **Real risk if it writes anything back**: the prototype's "Cap it" / "Set a $60/week limit" actions imply writing a spending limit somewhere. If that write path touches `BudgetCategory` or `Budget`, a bug there *would* affect the existing budgeting feature that other users already depend on — this is the highest-blast-radius part of the feature and should be scoped/tested separately from the read-only insight/display logic.
- **Shared job queue**: a new scheduled job runs on the same Sidekiq infrastructure as sync, import, and AI chat jobs. A poorly-scoped or slow weekly job (e.g., an N+1 query across all families) at Monday-morning scale could contend with or delay other scheduled/background work.
- **Email deliverability**: a new recurring mailer sent to the full user base is a different risk profile than the existing transactional mailers (password reset, invitations) which are low-volume and user-triggered. A bug that sends duplicate, malformed, or mistimed emails at scale is a real reputational risk, distinct from anything currently in the codebase.
- **Family-level data shown to the wrong context**: since budgets/categories are family-scoped, not user-scoped, a personalization bug could show one family member's spending pattern framed as if it were "yours" in a way that surprises multi-user families — worth explicitly testing.

---

## 3. What Would Affect How I Write the Spec

### Data that does not exist yet

- **Savings goals entirely.** There is no goal model, no target amount, no target date, no progress-tracking field anywhere in this schema. The "savings goal progress" section of the feature has *no existing data model to build on* — this is new schema, new UI for goal creation, and new logic for progress calculation, not just a new insight surfaced from existing data.
- **Push notification infrastructure.** `MobileDevice` handles OAuth tokens for the mobile app's API access, but there's no APNs/FCM device-token storage or push-sending integration. If "nudge" delivery is meant to include push notifications (not just email + in-app), that's net-new infrastructure, not a small addition.
- **Any concept of "spending trend vs. personal average" that's pre-computed/cached.** `median_expense`/`avg_expense` exist as on-demand calculated methods on `IncomeStatement`, not stored/cached values. Running these for every family every Monday at scale is a different performance question than running them on-demand for one user viewing their own budget page.

### Existing constraints to call out in the ticket

- **No services layer, by explicit team convention.** Don't spec this as a "WeeklySummaryService" — this team's stated convention is business logic in models/POROs, not a services directory. Phrasing the ticket to match their own conventions will get a faster, less contentious review.
- **Minimize-dependencies convention.** If push notifications are in scope, expect pushback on adding a new dependency (e.g., a push-notification gem/service) unless there's a strong stated reason — worth pre-empting in the spec rather than having it surface as a review blocker.
- **Family-level, not user-level, data model.** Decide and state explicitly in the spec whether "personalized" means per-family or per-user — the existing model defaults to family-level budgets/categories, so per-user personalization is a deviation from the grain of the existing data model, not a natural extension of it.
- **This repository is archived/no longer actively maintained** (per the README). If this is genuinely the codebase in question (vs. Nudge's own, similarly-structured app), that's a fundamental planning constraint, not a detail — confirm this before treating any of the above as buildable roadmap.

### The one thing engineers will ask before kickoff

**"Is the savings goal a net-new feature we're building the data model for, or are you assuming it already exists?"**

Everything else in the feature (spending insight, category-level nudge) maps reasonably well onto existing models and could plausibly be scoped as "extend what's there." The goal-progress piece is the one place where the spec's assumed scope and the actual codebase diverge sharply — if the ticket doesn't clarify this upfront, expect the estimate to swing wildly depending on which assumption the engineer defaults to. Answer this before scheduling kickoff, not during it.
