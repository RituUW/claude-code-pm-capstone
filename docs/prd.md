# PRD — Personalized Weekly Summary

## Problem Statement

30-day retention dropped from 44% to 37%. Users get real value from the initial spending breakdown, but the app goes static after week 1 — nothing changes regardless of tenure, so users conclude there's nothing left to see and stop opening it. This is the top NPS complaint (5 of 10 verbatims) and shows up independently in user interviews (Tom, Amara). The weekly summary email already gets a 22% open rate, but the in-app experience doesn't continue what the email starts — that disconnect is part of the problem, not a separate one.

## User

**Who:** Someone who connected an account weeks ago, saw the spending breakdown, and has gone passive — opening rarely or not at all.
**Job to be done:** Understand where their money went this week and take one concrete action on it — not just review data.

## Goals and Non-Goals

**Goals:**
- Give users a reason to open the app weekly, tied to their own real data.
- Surface one non-obvious insight per week — the category with the biggest % deviation from the user's own historical average — not a restatement of what they already suspect.
- Pair that insight with one explicit, actionable nudge. Require a decision (act or dismiss), not passive display.
- Proactively surface savings-goal progress when a goal exists — closes the "you set a goal and I never mentioned it again" gap from NPS.
- Continue the weekly email's story in-app, so clicking through doesn't feel like a different product.

**Non-Goals (v1):**
- Multi-goal support — single goal only.
- Mid-week proactive alerting — cut to backlog, no new scheduled job for this yet.
- Deep, multi-month pattern recognition (the mechanism that retains long-tenured power users) — that's a longer-horizon bet; this feature's job is week-1-through-week-4 retention specifically, not replicating that.
- New data integrations — existing data only.

## Success Metrics

- **Primary:** 30-day retention rate (baseline: 37%).
- **Supporting:** weekly summary open rate (trend across sends, not just send 1), nudge action-taken rate, average sessions/user per week.

## User Stories

1. As a user who hasn't opened the app in over a week, I get an email highlighting one specific thing that changed, so I have a concrete reason to come back instead of a generic reminder.
2. As a user clicking through from that email, I see the same headline continued in-app, so the experience doesn't feel disconnected from what I just read.
3. As a user viewing my summary, the top insight is something I didn't already know — not last week's category again — so the app doesn't feel like it's just confirming my suspicions.
4. As a user with a savings goal, I see progress on it every week without asking, so I don't feel like Nudge forgot what I told it.
5. As a user given a nudge (e.g., "cap category X at $Y/week"), I can act on it or dismiss it in one tap, so the app is asking something of me, not just displaying something at me.

## Open Questions

- What's the actual deviation threshold for "novel"? (30-40% was an engineering placeholder, not a validated number.)
- What do the flat-week and cold-start fallback states actually look like — specified, but not yet designed or tested.
- Does the ranking logic need an explicit variety rule (don't repeat the same top category two weeks running), and who owns that threshold?
- Is bottom-nav / on-demand access the final IA decision, or still open?
- Unresolved: whether repeated weekly summaries actually bridge users to the deeper personalization payoff that retains long-tenured users, or just delay churn without closing that gap.
