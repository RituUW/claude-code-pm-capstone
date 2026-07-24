# Nudge — Project Context

## Product

Nudge is a consumer personal finance app that helps people build better money habits. Users connect their bank accounts, see where their money is going, set savings goals, and get nudges to stay on track.

- Launched 4 years ago
- Series B funded ($42M)
- 2.1 million registered users
- 340,000 monthly active users (MAU)
- Growing at 28% year over year on MAU

## Role & Team

PM on the Engage squad — owns everything related to keeping users active after they connect their first account: the home feed, weekly money summaries, savings nudges, and push notifications.

Triad:
- Raj — Senior Engineer
- Lena — Product Designer
- (me) — PM

Reports to Marcus, Head of Product. Detailed stakeholder profiles (confirmed facts vs. assumed defaults, clearly separated) live in `stakeholders/raj.md`, `lena.md`, `marcus.md`.

## The Core Problem

30-day retention (percentage of users still active 30 days after connecting their first account) dropped from 44% to 37% over the last two quarters. Users get real value from the initial spending breakdown, but the app goes static after week 1 — nothing changes regardless of tenure, so users conclude there's nothing left to see and stop coming back before any deeper value emerges.

**Supporting evidence (fully converged across 4 independent sources — see `research/`):**
- Users who don't set a savings goal in week 1 churn at roughly 2x the rate of users who do.
- The home feed doesn't change based on user activity or tenure.
- The weekly summary email has a 22% open rate, but the in-app experience never continued its story — a real, named disconnect.
- Users want novel insights, not confirmation of what they already suspect; no competitor (including the closest, Cleo) sustains a sense of novelty over time.

## Key Tension & Constraints

- **Key tension:** engagement volume vs. notification fatigue — more nudges risk unsubscribes even as they aim to pull users back.
- **Constraint:** use existing data only, no new integrations.
- **Trust risk:** competitors (Rocket Money, Cleo) have lost real goodwill over fee transparency and over-promised features (Cleo: $17M FTC settlement) — a cautionary pattern to avoid, not a target to replicate.

## Current Status (as of this update)

We are past discovery. The team is asking Marcus to fund a properly powered follow-up test — not a full rollout yet.

- **Direction approved:** personalized weekly summary (top insight, one actionable nudge, savings goal progress) — see `docs/decision-brief.md`.
- **Prototype built and iterated 3x** (`prototype/index.html` → `v2.html` → `v3.html`; **v3 is the latest and most complete**). Tested via 2 live usability sessions plus a synthetic persona-based review. The 3 blocking issues found in QA (goal-count mismatch, mid-week alert over-promise, ambiguous transaction total — see `docs/qa-checklist.md`) have been fixed directly in `v3.html`.
- **Spec pressure-tested with Raj** (`docs/spec-readiness.md`): single savings goal for v1, mid-week alerting cut to backlog, "novel insight" defined as biggest % deviation from a user's own historical average, 3-tier fallback design (strong insight / flat week / cold start) — designed but not yet built or tested.
- **Design-reviewed with Lena** (`docs/design-review.md`): biggest open risk is shipping the happy-path state only, without validating the flat-week/cold-start states first.
- **Real pilot run** (week-5 cohort, n=50/variant): day-30 retention 36% (treatment) vs. 22% (control) — a real 14-point lift, driven by a measurable mechanism (avg. sessions/user restored from ~3.7 to ~5.3) — but **not statistically significant at this sample size** (p ≈ 0.12). Full analysis: `data/metric-findings.md`, `data/metric-diagnosis.md`.
- **Full test designed:** ~1,160 users/variant, ~7.3 weeks, fits within an 8-week window — see `data/experiment-design.md`.
- **PRD written** (`docs/prd.md`) and pressure-tested against 3 reviewer personas (`docs/objection-log.md`) — the identified top risk was presenting this as "ready to build" before the full test runs, which could cause Marcus to pause resourcing entirely.
- **Quarterly review deck built** (`docs/presentation.md` + `presentation-notes.md`) — the ask is explicitly to fund the full test, not a company-wide rollout, addressing that risk directly.

**Open blockers:**
- Awaiting Marcus's resourcing decision on the full test (ask made in the quarterly deck).
- Raj flagged a ranking-logic data backfill with an estimate still TBD.
- **Unresolved data-provenance question:** it's not confirmed anywhere in this workspace whether `Copy of Nudge Dataset.xlsx` (source for all pilot/experiment analysis) is real production data or an illustrative/exercise dataset. Now flagged with an inline warning at the top of every file that depends on it (`data/metric-findings.md`, `metric-diagnosis.md`, `experiment-design.md`, `docs/recommendation-memo.md`, `docs/presentation.md`/`presentation-notes.md`) — confirm before treating any pilot-derived number as a real business result.

## Capstone Review

A full review of everything built, with per-artifact confidence scoring, proposed fixes, and a portable prompt for reusing this workflow on a different product, is in `docs/capstone-session.md`. Read it before assuming any artifact here is more validated than it actually is — several (v3 prototype changes, the Raj/Lena stakeholder reviews) are simulated, not confirmed with real people yet.

## Where Things Live

- `change_log.md` — full chronological history; the most detailed source of truth if this summary isn't enough.
- `strategy.md` — detailed running log of research findings, more granular than this file.
- `project.md` — early-stage overview; explicitly marked stale inline, pointing back to this file — this CLAUDE.md file is now the authoritative current-status summary.
- `archive/` — superseded early drafts (e.g., `PRD Skeleton.md`, marked superseded inline; current PRD is `docs/prd.md`).
- `docs/` — all deliverables: decision brief, hypothesis synthesis, PM brief, spec readiness, design review, QA checklist, PRD, objection log, recommendation memo, quarterly deck + speaker notes, triad session prep, reference-codebase summary, prototype iteration log.
- `research/` — interview synthesis (Priya/Tom/Amara), NPS analysis, competitive matrix (5 apps), competitive Reddit sentiment.
- `data/` — pilot analysis (`metric-findings.md`, `metric-diagnosis.md`) and the full-test design (`experiment-design.md`) — each flagged inline with the unconfirmed data-provenance warning above. Note: the underlying source dataset lives outside this workspace, in `~/Downloads/Copy of Nudge Dataset.xlsx`, not copied in here.
- `prototype/` — `index.html` (v1), `v2.html`, `v3.html` (latest, all known QA bugs fixed), `README.md`.
- `stakeholders/` — `raj.md`, `lena.md`, `marcus.md` — profiles separating confirmed workspace facts from assumed defaults, used for prep and roleplay.
- `skills/` — `weekly-status.md`, `friday-status.md`, `research-synthesis.md`, `competitive-pulse.md` — reusable, single-prompt PM workflows.
- `agents/monday-retention.md` — a built-and-verified weekly metrics digest script + Slack template, with a documented path to production.
- `docs/capstone-session.md` — full workspace review: what's built, confidence per artifact, proposed fixes, and a portable prompt to recreate this workflow for a different product.
