# Engage v2 — Learning Synthesis & Hypothesis

Source: `change_log.md` (full discovery-through-prototyping history) and `docs/decision-brief.md`.

## What We Know

- 30-day retention dropped from 44% to 37% over the last two quarters, and the app goes static after week 1 — the home feed and dashboard don't change with tenure, and users interpret "nothing new" as "nothing left to offer." This is corroborated across NPS (5/10 verbatims), user interviews (Tom, Amara), and is the single biggest driver of disengagement identified.
- Users who set a savings goal in week 1 churn at roughly half the rate of those who don't (Raj's original data pull).
- The weekly summary email has a 22% open rate, but the in-app experience doesn't continue its story after click-through — a real, named disconnect, not a hypothesis.
- Users want novel insights, not confirmation of what they already suspect — validated internally (NPS) and externally (no competitor, including Cleo which comes closest, sustains a sense of novelty over time; Reddit sentiment shows even Cleo's personality-driven engagement "gets old" within about a week).
- Explicit-action nudges (a specific number + a real button, not a passive display) drive stated intent to act. In live usability testing (Round 1, prototype v1), both participants said they'd act on the nudge, citing its specificity as the reason.
- Entry-point/wayfinding clarity matters: 100% of live Round 1 participants couldn't say how they'd naturally arrive at the experience, even though they understood it once they saw it.
- Trust and monetization over-promising is a real reputational risk observed in competitors (Rocket Money's fee transparency complaints, Cleo's $17M FTC settlement) — not a target to replicate as we add proactive features.

## What We're Assuming

- That week-1 retention needs a fundamentally different mechanism than the deep, months-long personalization that retains long-tenured users (Priya) — this is a logical inference from the interview synthesis, not something we've directly tested.
- That mandatory savings-goal follow-up, once built into real ranking logic, will meaningfully reduce churn — currently only supported by NPS complaints about goals being ignored and reactions to a hardcoded mock, not by any test of the real mechanism.
- That the personalized weekly summary is the right lever, chosen over the other two options considered (more aggressive onboarding goal-prompting, tenure-varying home feed) — this was a reasoned decision, not something tested head-to-head.
- That the v3 prototype changes (transaction drill-down, multi-goal support, mid-week proactive framing, softened nudge tone) will land with real users — these were built in response to a single synthetic, AI-roleplayed interview session, not live participants, and are explicitly flagged as unvalidated in the iteration log.
- That users who click "Cap it" or "Set a limit" in a real product would actually change spending behavior, not just express intent in a low-stakes prototype setting.

## What We Still Don't Know

- Whether stated intent to act ("I would set that limit") translates into sustained behavior change over weeks or months — we have no data past a single-session reaction.
- Whether the real ranking logic (not yet built) can reliably surface accurate, genuinely novel insights at production scale — this is unbuilt engineering work with real technical risk, not yet tested.
- Whether this feature moves 30-day retention at all, and by how much — everything to date is prototype-stage qualitative testing (2 live participants, 3 synthetic personas), not a live product test.
- Whether nudge/notification fatigue emerges once this ships broadly — the engagement-volume-vs-fatigue tension was flagged as a key tension from day one and remains unresolved.
- Whether accurate multi-goal personalization is achievable within the "no new integrations, existing data only" constraint — multi-goal support in v3 is new scope beyond the original PM brief and hasn't been checked against that constraint.
- Whether the weekly summary successfully bridges users to the point where Priya's deep, ~3-month personalization payoff kicks in, or whether it only delays churn without closing that gap.
- Reddit sentiment for one competitor (Copilot Money) was thin due to search tooling limits — our competitive picture has a real, acknowledged gap there.

## Hypothesis Statement

We believe that **a personalized weekly in-app summary — a novel, non-obvious spending insight, one explicit-action nudge, and proactive savings-goal follow-up, continuing the story the weekly email already starts** — will deliver **a concrete, recurring reason to return to the app and take action** for Nudge users in their first 30 days, as measured by 30-day retention rate.
