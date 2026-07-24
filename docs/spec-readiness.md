# Spec Readiness — Nudge Weekly Summary

Method: simulated pre-kickoff review with Raj (Eng), roleplayed against `docs/decision-brief.md`, `docs/pm-brief.md`, `docs/hypothesis.md`, `docs/triad-session.md`, `docs/codebase-summary.md`, `docs/iteration-log.md`, and `stakeholders/raj.md`.

## 1. Spec Readiness Summary

**What was solid going in:**
- The *why* — decision-brief.md gives a clear, multi-source-validated problem statement and a specific recommended action. Raj didn't push back on this at all; it wasn't in question.
- The savings-goal data model turned out to be far less risky than the docs implied — the feature already exists in production, single-goal. This was the single biggest unknown flagged across `docs/hypothesis.md` and `stakeholders/raj.md`, and it resolved cleanly in one question.
- Instrumentation/analytics is already planned — the one open question in Raj's own stated profile ("how will we know if this is working") turned out to already have an answer, it just wasn't written down anywhere the spec review could see it.

**What needed work (and got resolved in this session):**
- **No actual spec existed.** What was in `docs/` was a decision brief, a one-paragraph PM brief, a hypothesis doc, and a prototype — none of which is a buildable spec with acceptance criteria. This review effectively *became* the first draft of the spec.
- **"Novel insight" had no operational definition.** Now defined: biggest % deviation from the user's own historical category average.
- **No fallback for the case where nothing is novel** (flat spending week) or where there's no historical average yet (new user, cold start). Both mocked scenarios in the prototype assumed a convenient anomaly every time. Now resolved as a 3-tier model (see rewrite below).
- **Multi-goal scope creep wasn't caught until this review.** The v3 prototype quietly expanded from "a savings goal" (singular, per the original PM brief) to two goals. Scope is now explicitly pulled back to one goal for v1.
- **Mid-week alerting was implied in prototype copy with no real mechanism behind it**, and would have required a second scheduled job, partial-week threshold logic, and dedup logic with no push infrastructure to lean on. Now explicitly cut to backlog for v1.
- **No stated threshold number** for what counts as a "novel" deviation (30-40% was floated as an engineering placeholder, not a validated product number) — this remains a genuinely open item, flagged below, not resolved in this session.

## 2. Rewrite of the Unclear Sections

### Savings Goal (was: undefined data model, unclear single vs. multi-goal)

> **Data source:** existing savings-goal feature (already in production). **Scope for v1: single goal only** — do not build or design for multiple concurrent goals in this release. Display target amount, current progress, and this-week's contribution, using existing goal data as-is.

### Top Insight — Ranking Logic (was: "novel insights, not confirmatory ones," no operational definition)

> **Definition of "novel":** the category with the largest percentage deviation from the user's own historical average spending in that category.
>
> **Fallback logic — three tiers:**
> 1. **Normal case:** a category deviation clears an agreed threshold (placeholder: 30-40% above the user's own average — **needs a validated number, not an engineering guess, before build**) → show it as the top insight, as in the current prototype.
> 2. **Flat week (no category clears the threshold):** do not show an empty or degraded insight card. Promote savings-goal progress to the top slot instead, framed positively (e.g., "Right in line with your usual spending this week — your [Goal] is up $X"). Uses existing goal data; no new computation required.
> 3. **Cold start (not enough transaction history to compute a personal average):** do not make a comparative claim. Fall back to either a plain total-spend statement or, if the user hasn't set a goal yet, a goal-setting nudge — the single stat most correlated with reduced churn in existing data.
>
> **Design note:** tiers 2 and 3 need real copy and layout from design — do not ship with engineering-default placeholder text.

### Mid-Week Alerting (was: implied in prototype UI copy, no mechanism)

> **Out of scope for v1 — backlog.** The v3 prototype's "we'll flag this again mid-week" copy should be removed or clearly marked as future-looking before this spec goes to design/build. Revisit only after the core weekly summary ships and the team wants to invest in a second scheduled job, partial-week evaluation logic, and a delivery/dedup strategy (no push infrastructure currently exists — email or in-app only).

### Success Measurement (was: undocumented, though apparently already planned)

> **Instrumentation required at launch** (confirmed already planned, needs to be explicitly written into the spec so it's not assumed/lost):
> - Event: weekly summary opened
> - Event: nudge action taken (e.g., limit set) vs. dismissed
> - Cohorting: users who received/opened a summary vs. those who didn't, to allow actual retention comparison — not just aggregate before/after tracking

## 3. Async Slack Message — Confirm Scope Before Sprint Kickoff

---

**To: Raj**

Hey — recapping where we landed on the weekly summary spec so we're aligned before kickoff. Confirming scope for v1:

- **Savings goal:** using the existing goal feature, **single goal only** — no multi-goal in this release.
- **Top insight:** defined as biggest % deviation from the user's own historical category average. Still need to lock the actual threshold number (I know 30-40% was your placeholder, not a validated number — I'll bring a number or a way to test it before we finalize).
- **Fallback states:** 3 tiers — normal (deviation insight), flat week (promote goal progress instead), cold start (no comparison, total spend or goal-setting nudge). I'll get real copy/layout for tiers 2 and 3 from Lena before this goes into a ticket — not shipping with placeholder text.
- **Mid-week alerting:** cutting from v1 per your recommendation, moving to backlog. Will strip the "mid-week" copy from the prototype before it goes further.
- **Instrumentation:** confirming open + nudge-action events and open/non-open cohorting are in the plan — can you point me to wherever that's actually specced so I can reference it in the ticket instead of re-describing it?

Let me know if I mischaracterized any of this before I write it up as a ticket. Otherwise, aiming to bring this to sprint planning as-is.

---
