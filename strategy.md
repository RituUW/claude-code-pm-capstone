# Nudge Engage v2 — Strategy

## Hypothesis: Recovering the 7-Point Retention Drop

30-day retention dropped from 44% to 37% over the last two quarters. Working hypothesis: users go passive because the app stops feeling relevant after the first "aha moment" (the initial spending breakdown). After that, Nudge doesn't give users a reason to come back that feels personal, timely, or actionable.

### Supporting evidence

- Users who don't set a savings goal in week 1 churn at roughly 2x the rate of users who do.
- User research: users who connect their account and view the dashboard don't have a clear next step after the spending breakdown.
- The home feed doesn't change based on user activity or tenure — same experience whether a user opened the app yesterday or three weeks ago.
- The weekly summary email has a 22% open rate, but the in-app experience doesn't continue the story the email starts after click-through — creates a jarring disconnect.

### Key tension

Engagement volume vs. notification fatigue — pushing more nudges/notifications risks annoying users and driving unsubscribes or churn, even as it aims to pull them back.

### Direction being explored

A personalized weekly in-app summary screen — top insight for the week, one actionable nudge based on the user's actual patterns, and progress toward their savings goal (if set). Technically doable with existing infrastructure; no new data sources needed. Main lift is building new ranking logic to determine which insight to surface.

### Open decision

What actually brings users back after week 1 — not yet resolved whether it's the personalized weekly summary screen, more aggressive savings-goal prompting in onboarding, home feed changes, or some combination.

### Interview synthesis findings (see [research/interview-synthesis.md](research/interview-synthesis.md))

- The deep pattern-recognition personalization that drives long-term stickiness (per an 18-month power user) takes roughly 3 months to develop — but users are deciding whether Nudge is "working" within days to weeks. Week-1 retention is not a scaled-down version of the long-term retention problem; it likely needs a different mechanism.
- A churned user (5 weeks) and a new user (10 days) both independently described the app as static after the initial spending breakdown, with no next step to act on.
- Possible tension to resolve: the power user was retained by Nudge *passively surfacing* insights over time, while the churned user wanted the app to *actively ask something* of him. Early-tenure users may need active prompting/asks; long-tenure users may be better served by quiet, passive insight delivery. This could inform how the personalized weekly summary should behave differently by tenure.
- Implication for Engage v2: week 1 likely needs a concrete, actionable next step delivered early — something to *do*, not just something to *see* — to bridge the gap before deeper personalization has enough data/time to mature.

### NPS feedback findings (see [research/nps-analysis.md](research/nps-analysis.md))

- Corroborates the core hypothesis: "no reason to return after week 1" was the single most common complaint across 10 raw verbatims (5 mentions), independent of the interview synthesis.
- Users want the ranking logic to prioritize **novel insights over confirmatory ones** — surfacing things they don't already know, not restating what they already suspect.
- Users who set a savings goal expect Nudge to **follow up on it proactively**; several report Nudge never mentions their goal again after it's set. Goal-progress follow-up should always be included in the weekly summary when a goal exists — low lift since the data already exists.
- Flagged risk: one user fully disabled notifications after a single "random-feeling" nudge (a $12 coffee purchase alert). Reinforces the existing engagement-volume-vs-notification-fatigue tension — supports being selective/high-signal in what gets surfaced rather than increasing volume.

### Competitive validation (see [research/competitive-matrix.md](research/competitive-matrix.md))

Analyzed 5 relevant competitors (YNAB, Monarch Money, Rocket Money, Copilot Money, Cleo). None of them own two gaps that map directly onto Nudge's own research findings:

- **Gap 1 — Passive, low-effort personalization that still feels novel.** YNAB's engagement requires active weekly budgeting discipline; Cleo achieves personal-feeling engagement but only through a chat/personality layer. No competitor delivers a fully passive experience where the app surfaces something new and personal each week with zero required user maintenance — this is the core bet of the personalized weekly summary direction.
- **Gap 2 — Proactive follow-through on a user's specific stated goal.** Every competitor tracks goals or net worth in some form, but none clearly closes the loop with recurring, structured "here's where you stand on the thing you told us you cared about" follow-up. Matches the goal-tracking gap Nudge users already flagged in NPS feedback.

This is independent, external validation (not just internal user complaints) that both gaps represent real white space, not just something Nudge happens to be missing.

### Reddit sentiment check (see [research/competitive-reddit.md](research/competitive-reddit.md))

- **Gap 1 confirmed with direct evidence:** Cleo — the competitor that comes closest to "personal-feeling" engagement via its chat personality — has users explicitly reporting the novelty "gets old" within about a week. No competitor's community sentiment shows users praising *sustained* novelty over time. Reinforces that passive, low-effort personalization that stays fresh is genuinely open white space.
- **Gap 2 (goal follow-through) not yet externally validated:** No competitor's Reddit sentiment directly praised or criticized goal follow-through specifically. This gap still rests primarily on Nudge's own NPS/interview data — worth a more targeted search if we want external confirmation before betting heavily on it.
- **New risk surfaced (not a gap to fill, a trap to avoid):** Trust/monetization friction is a recurring theme across competitors — Rocket Money's fee transparency and cancellation issues, Cleo's FTC settlement over exaggerated cash-advance claims. As Engage v2 builds out proactive nudges and goal follow-up, be unambiguously honest about what's being delivered — competitors have lost significant community goodwill over perceived over-promising.
