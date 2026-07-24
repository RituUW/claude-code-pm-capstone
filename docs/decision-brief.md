# Decision Brief: Engage v2 — Post-Onboarding Retention

Prepared for: Marcus, Head of Product
From: Engage squad (PM)

## Situation

30-day retention has dropped from 44% to 37% over the last two quarters: users respond well to the initial spending breakdown, but the app gives them no reason to return once that first "aha moment" passes. This isn't a hypothesis anymore — it's independently confirmed by user interviews, NPS feedback, and competitive research.

## Key Findings

- **The app goes static after week 1, and that's the single biggest driver of disengagement.** Interviews and NPS agree: the home feed and dashboard don't change with tenure, so users interpret "nothing new" as "nothing left to offer" and stop opening the app (NPS: 5/10 verbatims; interviews: Tom and Amara both independently describe this).
- **Real personalization takes months to build, but users decide within days to weeks whether Nudge is "working."** Priya's deep pattern-recognition payoff took ~3 months; Tom churned at 5 weeks, Amara was already stalling at 10 days. Week-1 retention needs an early, manufactured sense of momentum — not a scaled-down version of the mature personalization engine (interview synthesis).
- **Users want to be told something new, not have their suspicions confirmed — and want proactive goal follow-up.** NPS: users explicitly want novel insights over confirmatory ones, and multiple users report Nudge never mentions the savings goal they set. **Reddit confirms the novelty point directly**: even Cleo, the competitor closest to "personal-feeling" engagement via chat personality, has users reporting that novelty "gets old" within about a week — no competitor sustains it. Goal follow-through, by contrast, is validated only by our own data — no competitor's Reddit sentiment confirms or contradicts it, so treat it as lower-confidence.
- **No competitor owns passive, low-effort personalization that stays fresh over time.** YNAB retains users but demands active weekly discipline; Cleo achieves personal-feeling engagement only through a chat relationship that visibly decays. This is genuine, externally-validated white space for Nudge's proposed weekly summary — provided it requires zero maintenance from the user.
- **Trust/monetization friction is a real risk to avoid, not a target to replicate.** Reddit sentiment shows Rocket Money and Cleo losing significant goodwill over fee transparency and over-promised features (Cleo faced a $17M FTC settlement on exactly this). As we add proactive nudges and goal follow-up, over-promising what the product delivers is the fastest way to convert this initiative into a trust problem instead of a retention win.

## Options Considered

1. **Personalized weekly in-app summary** (top insight, one actionable nudge, savings goal progress) — the direction already in early exploration. Technically feasible with existing data; requires new ranking logic.
2. **More aggressive savings-goal prompting in onboarding** — directly targets the finding that goal-setters churn at half the rate of non-setters, but doesn't address the static-experience problem for users who already set a goal.
3. **Home feed changes that vary by tenure/activity** — addresses the "static feed" complaint directly but risks becoming a system-generated feed without the specific, evidence-backed hooks (novelty, goal follow-up) users are asking for.

## Recommended Action

Move forward with the personalized weekly in-app summary as the primary Engage v2 bet, with ranking logic explicitly weighted toward novel (not confirmatory) insights and mandatory savings-goal follow-up when a goal exists.

## Why Now

Four independent sources — user interviews, NPS feedback, structured competitive research, and real-user Reddit sentiment — converge on the same root cause and the same white space; the risk of further delay is continued erosion of the 37% baseline while a validated, technically feasible direction sits unbuilt.
