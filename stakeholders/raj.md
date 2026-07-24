# Stakeholder Profile — Raj

## Role

- **From workspace (`project.md`):** Senior Engineer on the Engage triad, "assessing technical feasibility of solution directions."
- **From default profile (used to fill gap):** Positioned as owning technical architecture, sprint scope, and feasibility decisions for the squad. *Note: this is a title/scope step up from "Senior Engineer" in the workspace record — worth confirming which is current before you rely on it, especially if it affects who has final say on scope calls.*

## What's Directly Documented About Raj (from workspace files)

- **Originated the key data finding that shaped the whole initiative:** identified that users who don't set a savings goal in week 1 churn at roughly 2x the rate of those who do (`change_log.md`, Day 1). This is described elsewhere as "Raj's original data pull" (`docs/hypothesis.md`) — he's the source of one of the most load-bearing stats in the entire strategy.
- **Confirmed technical feasibility** of the personalized weekly summary direction using existing data sources, and scoped that it requires new ranking logic (`change_log.md`, Day 1).
- **Owns the ranking logic build.** The prototype's data is explicitly hardcoded "no backend or ranking logic yet; that remains Raj's scoped work" (`change_log.md`, Day 2).
- **Unresolved data-model question tied to him:** the savings-goal field's data model isn't clarified yet — and independently, a reference-codebase review (`docs/codebase-summary.md`) found no savings-goal model exists at all in a comparable finance app, which suggests this may be a bigger open question than "clarify a field" — it could be "design the whole feature."
- **Specific feasibility questions queued for him** (`docs/triad-session.md`): whether the "novel insight" ranking logic is buildable with existing data, whether multi-goal support is a bigger lift than it looks, whether transaction drill-down needs new infra, what mid-week proactive alerting would take vs. the current weekly-batch cadence, and rough sizing of MVP vs. backlog.

## Filled from Default Profile (not yet confirmed in workspace — flagged as inferred)

- **Pushes back on:** underspecified requirements, scope that grows mid-sprint, anything touching the data pipeline without a clear rollback plan.
- **Needs before saying yes:** clear acceptance criteria, edge cases called out upfront, a clear answer to "what does done look like."
- **Has asked before, unanswered:** "How will we know if this is working after it ships?" and "What happens if the user has no transactions this week?" — neither has a documented answer anywhere in the workspace yet.
- **Communication preference:** async-first, short messages, bullets over paragraphs, dislikes being surprised in standups.
- **Open item:** still waiting on data-model clarification for the savings-goal field.

## What Would Most Change How You Prepare for Him

The empty-state question ("what happens if the user has no transactions this week?") and the savings-goal data-model gap are actually the same category of risk, and the codebase review independently surfaced it too — this isn't a minor open item, it's the thing most likely to blow up sprint scope if you walk into a conversation with him without an answer.
