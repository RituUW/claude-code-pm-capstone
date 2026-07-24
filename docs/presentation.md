# Quarterly Review — Nudge Engage v2 — Deck Structure

> **⚠️ Data provenance unconfirmed** — see `data/metric-findings.md`. The pilot numbers in Slide 4 have not been verified as real production data. Confirm before presenting this to Marcus.

## Slide 1 — The Problem

- **The number:** 30-day retention has dropped from 44% to 37% over the last two quarters.
- **The insight:** Users get real value on day one — the initial spending breakdown lands — but the app goes static after week 1, so they stop coming back before any deeper value has a chance to emerge.

## Slide 2 — Why Now

- **What changed:** We moved from a hunch to converged, independently-confirmed evidence this quarter — user interviews, NPS feedback, competitive research, and now an actual randomized pilot with real numbers, not just qualitative signal.
- **What we learned:** We can now name the specific failure mode with data — average sessions per user eroding cohort over cohort (5.74 → 3.69) — and we have a candidate fix that's been tested, at small scale, against that exact mechanism.

## Slide 3 — The Proposal

- **What it is:** A personalized weekly summary — one non-obvious spending insight, one explicit actionable nudge, proactive savings-goal progress — continuing the story the weekly email already starts (it has a 22% open rate; the app just never continued it).
- **What it isn't:** Not multi-goal support. Not mid-week alerting. Not the deep, multi-month personalization engine that retains long-tenured power users — that's a longer-horizon bet, not this quarter's scope. Not built on any new data or integrations.

## Slide 4 — Evidence

- **Prototype:** Three iterations, usability-tested with real participants — users understood the core insight unprompted and said, unprompted, that they'd act on it.
- **User voice:** Tom (churned): *"I switched to YNAB because at least that feels like it is asking something of me."* Amara (new user): *"I have been waiting for Nudge to tell me the next step."* NPS: *"If I got one really useful insight a week I would open it every week."*
- **Data:** Pilot (n=50/variant): day-30 retention 36% (treatment) vs. 22% (control); day-7 76% vs. 46%; session mechanism restored to 5.28 vs. control's 3.68. Promising and mechanistically well-explained — **and not yet statistically significant at this sample size** (p ≈ 0.12).

## Slide 5 — The Plan

- **Timeline:** Run a properly powered follow-up test — ~1,160 users per variant, enrolled across 3 cohort weeks, ~7.3 weeks total — fits inside an 8-week window.
- **Milestones:** Enroll (weeks 1-3) → monitor 3 weekly leading indicators (summary open-rate trend, sessions/user, day-7 retention gap) → decision gate around week 7-8.
- **Risks:** A ranking-logic data backfill Raj flagged has an estimate still TBD and could shift timing; a real platform gap surfaced in the pilot (iOS benefits strongly, Android doesn't) needs parallel investigation; the true effect could be smaller than the pilot's 14-point lift once measured at scale.

## Slide 6 — The Ask

- **Approve resourcing for the full, properly powered test** — not a full company-wide rollout yet — with sign-off needed by our next 1:1 so enrollment can start with the next cohort week rather than losing another cycle.
- **Keep the already-scoped engineering work moving in parallel** (ranking logic groundwork, spec items not gated on the bigger test's outcome) so the quarter isn't lost waiting on a single decision gate.
