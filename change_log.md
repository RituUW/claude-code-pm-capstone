# Nudge Engage v2 — Change Log

## Day 1 — 2026-07-07 — Discovery phase kickoff

- Retention numbers reviewed: 30-day retention at 37%, down from 44% last quarter.
- Team (Marcus, Raj, Lena, me) began investigating root cause via Slack thread.
- Raj identified correlation: users who don't set a savings goal in week 1 churn at ~2x the rate of those who do.
- Lena flagged from user research: no clear next step after spending breakdown; weekly summary email (22% open rate) doesn't carry through to the in-app experience.
- Working hypothesis formed: app stops feeling relevant after week 1, no personalized reason to return.
- Direction proposed by Lena: personalized weekly in-app summary screen (top insight, actionable nudge, savings goal progress). Raj confirmed technically feasible with existing data sources; requires new ranking logic.
- Marcus requested a written problem statement before Thursday's alignment meeting.
- PRD skeleton drafted (see [PRD Skeleton.md](PRD Skeleton.md)).
- project.md, strategy.md, change_log.md created to track ongoing work.
- Reusable skill created for weekly leadership status updates (see [skills/weekly-status.md](skills/weekly-status.md)): takes raw notes, outputs Shipped/In Progress/Blockers/Next Week sections, 3 bullets max each, plain language.
- Synthesized 3 user interviews (power user, churned user, new user) — see [research/interview-synthesis.md](research/interview-synthesis.md). Key finding: the personalization that drives long-term stickiness takes ~3 months to develop, but users are deciding within days-to-weeks whether Nudge is "working." Week-1 retention needs a manufactured sense of early momentum/actionability, not a scaled-down version of the mature personalization engine. Insight folded into strategy.md.
- Analyzed 10 raw NPS verbatims — see [research/nps-analysis.md](research/nps-analysis.md). Corroborates the core hypothesis: top complaint (5 mentions) is no reason to return after week 1. Two new, more specific findings for the ranking logic: prioritize novel insights over confirmatory ones, and always surface savings-goal progress follow-up when a goal exists. Findings folded into strategy.md.
- Completed competitive analysis of 5 relevant apps (YNAB, Monarch Money, Rocket Money, Copilot Money, Cleo) — see [research/competitive-matrix.md](research/competitive-matrix.md). Identified 2 white-space gaps for Nudge: (1) passive, low-effort personalization that feels novel without requiring active budgeting discipline or a chat relationship, and (2) proactive follow-through on a user's specific stated goal. Both gaps independently validate findings already in strategy.md from the interview synthesis and NPS analysis. Folded into strategy.md.
- Synthesized Reddit/community sentiment for the same 5 competitors — see [research/competitive-reddit.md](research/competitive-reddit.md). Gap 1 (passive, sustained-novelty personalization) holds up strongly: Cleo users explicitly report its personality-driven engagement "gets old" within about a week — evidence no competitor sustains novelty over time. Gap 2 (proactive goal follow-through) not contradicted but also not externally confirmed — still resting on internal NPS/interview data only. New signal surfaced: trust/monetization friction (Rocket Money fees, Cleo's FTC settlement over cash-advance and cancellation claims) — a reputational risk to avoid, not a gap to fill. Folded into strategy.md.
- Produced a 1-page decision brief for Marcus synthesizing all four research sources (interviews, NPS, competitive matrix, Reddit sentiment) — see [docs/decision-brief.md](docs/decision-brief.md). Recommends moving forward with the personalized weekly in-app summary as the primary Engage v2 bet, with ranking logic weighted toward novel insights and mandatory savings-goal follow-up.

## Day 2 — 2026-07-08 — First working prototype

- Built a working, interactive prototype of the personalized weekly summary for user testing — see [prototype/index.html](prototype/index.html) and [prototype/README.md](prototype/README.md). PM brief saved to [docs/pm-brief.md](docs/pm-brief.md).
- Flow: mock weekly summary email (fixing the email→app disconnect flagged in research) → in-app screen with top spending insight, a contextual nudge requiring explicit user action (not passive display, per the Tom/Priya tension from interviews), and savings-goal progress for a persona who already set a goal (directly testing the goal-follow-up gap from NPS).
- Nudge action simulates a specific state change (spending limit applied, visible progress bar, concrete projection) rather than a generic confirmation.
- All data is hardcoded for testing the concept — no backend or ranking logic yet; that remains Raj's scoped work.

## Day 2 (cont.) — 2026-07-08 — Round 1 usability sessions

- Ran usability sessions on the v1 prototype with 2 participants (Sonia, Ayo) — see [docs/iteration-log.md](docs/iteration-log.md).
- Working: both users correctly self-explained the top insight unprompted, and both said they'd act on it (cap spending / set the limit) — validates the core novel-insight + explicit-action bet.
- Top friction: (1) both users, unprompted, couldn't say how they'd naturally arrive at this experience — wayfinding/discoverability gap; (2) the nudge under-communicates scope/payoff at decision time (Ayo wants more than one category, Sonia wants the savings projection shown before committing, not a week later).
- Highest-priority fix shipped in [prototype/v2.html](prototype/v2.html): added a context strip under the app top bar ("Opened from your weekly email · refreshes every Monday morning") to resolve the wayfinding friction — the one issue raised by 100% of participants. Category-breadth and upfront-projection requests logged for a future round, not yet built.
- Priority updated: PM decided to bring forward Ayo's category-breadth request ahead of Sonia's upfront-projection request. Added a "More Places To Save" card to [prototype/v2.html](prototype/v2.html) (built directly into v2, no separate v3 file) showing Rideshare (+74%) and Subscriptions (+50%) vs. average with quick "Cap it" actions, plus Coffee Shops shown as on-track for contrast. Primary single-category nudge (Food Delivery) left unchanged so the next round can isolate the effect. See [docs/iteration-log.md](docs/iteration-log.md).

## Day 2 (cont.) — 2026-07-08 — Agentic persona interview on v2

- Ran a synthetic in-character interview against [prototype/v2.html](prototype/v2.html), playing all three research personas (Priya, Tom, Amara) through the same 4 questions used in the real usability sessions. No prototype changes made this round — findings only, logged here for the next iteration to action.
- **Priya:** Comprehension and motivation both strong (the goal-tied projection language, "6 weeks sooner," specifically resonated). New friction surfaced: the mock goal ("Emergency Fund") doesn't match her actual goal (Europe trip) — a personalization/mismatch risk when this becomes real, not hardcoded. Also wants the warning mid-week, before overspending happens, not after the fact on a weekly cadence.
- **Tom:** Understood the screen fine, but his real friction is upstream — whether he'd open the triggering email at all, given his history of abandoning finance apps. Notably, the explicit-action nudge design (a specific number + a button) partially won him over despite his skepticism, validating the earlier "explicit action, not passive" decision — but the framing ("so it doesn't creep back up") read as mildly lecturing to him, which risks his stated "don't lecture me" boundary.
- **Amara:** No comprehension issues, but flagged two trust-critical gaps: (1) she checks accounts regularly and questioned why this is only available via a weekly email trigger rather than on-demand, undermining her use case of replacing a manual spreadsheet system; (2) she immediately caught that the shown goal ("Emergency Fund") isn't a goal she actually has, and per her profile, a wrong-looking number is a product-ending trust break, not a minor issue.
- **Highest-risk finding:** Amara's goal-data mismatch reaction is the most consequential — it points to a real requirement (goal/insight personalization must be accurate, not hardcoded/generic) that matters most for exactly the trust-sensitive user segment most likely to actually replace a manual process with Nudge. Not yet actioned in the prototype; flagged for prioritization in the next round alongside Priya's mid-week timing request and Tom's tone concern.

## Day 2 (cont.) — 2026-07-08 — v3: acting on persona interview findings

- Built [prototype/v3.html](prototype/v3.html), addressing all three findings from the agentic persona interview round:
  - **Amara — trust/verification:** Top insight amount and each "More Places To Save" category row are now clickable and expand to show the underlying transactions, so a number is never presented without an audit trail.
  - **Amara — on-demand access:** Added a persistent bottom nav bar (Home / This Week / Goals / More) and updated the context strip to state the screen is available anytime under "This Week," not only reachable via the weekly email.
  - **Amara — goal mismatch / multi-goal:** Savings Goals card now shows two goals (Emergency Fund + Credit Card Payoff) instead of a single hardcoded goal, directly addressing her "this isn't even a goal I have" reaction.
  - **Priya — mid-week timing:** Added a subnote on the nudge card ("if you're trending above this mid-week, we'll flag it then too") and reworded the post-action projection to reflect proactive, not purely retrospective, monitoring.
  - **Tom — tone:** Reworded the nudge question from "...so it doesn't creep back up" to the neutral "Set a $60/week limit for food delivery?" — keeps the explicit, specific action but removes the lecturing framing.
- This is a synthesis of three personas' feedback into one prototype version rather than isolating a single variable — appropriate here since the findings came from a synthetic session, not a live usability round with real participants to re-test cleanly. Recommend the next live round re-test with real users to confirm these changes land as intended before treating them as validated.

## Day 2 (cont.) — 2026-07-08 — Learning synthesis and hypothesis statement

- Synthesized the full change log and decision brief into a know/assume/don't-know breakdown and a formal hypothesis statement — see [docs/hypothesis.md](docs/hypothesis.md).
- Know: static-app-after-week-1 as the core problem, goal-setter churn correlation, novelty-over-confirmation preference, explicit-action nudges driving stated intent, wayfinding gap, and trust/over-promising risk — all multi-source validated.
- Assume: that week-1 needs a different mechanism than long-term retention, that goal follow-up and the v3 changes will hold up with real users, and that this feature (vs. the other 2 options considered) is the right lever — none of these have been tested head-to-head or at scale.
- Don't know: whether stated intent converts to real behavior change, whether the ranking logic is buildable at production quality, whether this moves 30-day retention at all, whether nudge fatigue emerges at scale, and whether multi-goal support fits the "no new integrations" constraint.
- Hypothesis statement: "We believe that a personalized weekly in-app summary — a novel spending insight, one explicit-action nudge, and proactive savings-goal follow-up — will deliver a concrete, recurring reason to return to the app and take action for Nudge users in their first 30 days, as measured by 30-day retention rate."

## Day 2 (cont.) — 2026-07-08 — Triad session prep

- Prepared for a 30-min working session with Raj (Eng) and Lena (Design) to review the prototype before scoping real build work — see [docs/triad-session.md](docs/triad-session.md).
- Includes: session agenda (context recap, live v1→v3 walkthrough, feasibility questions for Raj, design/density questions for Lena, decisions to walk out with), a post-session alignment doc template (decisions, open questions, action items, next checkpoint), and the Slack invite message.
- Explicitly flagged in the agenda that v3's changes are unvalidated with live users (synthetic persona session only) — session goal is to decide what's in scope for a first build vs. backlog, not to treat v3 as final.

## Day 2 (cont.) — 2026-07-08 — Reference codebase tour (maybe-finance/maybe)

- Cloned and inspected the open-source `maybe-finance/maybe` Rails app as a reference codebase to sanity-check the weekly summary feature against a real personal-finance app's data model — see [docs/codebase-summary.md](docs/codebase-summary.md).
- **Biggest finding:** no savings-goal model exists in that codebase at all — `Budget`/`BudgetCategory` is a monthly spending plan, not a target-amount goal (e.g., "Europe trip," "Emergency Fund"). Flagged as the single question to resolve before any engineering kickoff, since it swings the estimate the most.
- Good news: "top spending insight vs. usual average" comparison logic largely already exists there (`IncomeStatement`/`BudgetCategory` compute `avg_expense`/`median_expense` per category) — useful precedent for scoping Nudge's own ranking logic.
- Also flagged: no recurring/digest mailer or weekly-cron pattern exists yet there (though a cron pattern to follow exists), and no push-notification infra (only OAuth device registration, not push tokens). Also noted that repo's README states it's no longer actively maintained — a planning caveat if it's meant to represent Nudge's actual codebase.

## Day 2 (cont.) — 2026-07-08 — Stakeholder profiles

- Created stakeholder profiles for Raj, Lena, and Marcus — see [stakeholders/raj.md](stakeholders/raj.md), [stakeholders/lena.md](stakeholders/lena.md), [stakeholders/marcus.md](stakeholders/marcus.md). Each separates what's directly documented in workspace files (change_log.md, project.md, decision-brief.md, triad-session.md, hypothesis.md, pm-brief.md) from defaults used to fill gaps.
- Key cross-cutting finding: Raj's open item (savings-goal data model) and Lena's open item (revisiting the goal progress bar after Amara's feedback) are the same underlying gap seen from two angles — flagged to resolve together rather than separately. Marcus has already approved the direction; the open gap with him is a specific retention-impact estimate and rollout timeline, not the concept itself.

## Day 2 (cont.) — 2026-07-08 — Simulated spec review with Raj

- Ran a roleplayed pre-kickoff spec review as Raj (using his stakeholder profile) against all of `docs/` — see [docs/spec-readiness.md](docs/spec-readiness.md).
- Surfaced that no real spec existed yet — only a decision brief, PM brief, hypothesis doc, and prototype. The review itself became the first spec draft.
- Resolved: savings-goal data model (exists, single-goal for v1, multi-goal cut from v3 scope), a working definition of "novel insight" (biggest % deviation from personal historical average), a 3-tier fallback design for flat-spending weeks and cold-start new users, mid-week alerting cut to backlog (per Raj's recommendation — would require a second scheduled job and no push infra exists to lean on), and confirmation that success instrumentation (open events, nudge-action events, cohorting) is already planned.
- Still open: the actual deviation-threshold number for "novel" (30-40% was Raj's engineering placeholder, not a validated product number) — flagged explicitly, not resolved. Async Slack message drafted to confirm scope with Raj before sprint kickoff.

## Day 2 (cont.) — 2026-07-08 — Simulated design review with Lena

- Ran a roleplayed design review as Lena (using her stakeholder profile) against `prototype/index.html`, `research/interview-synthesis.md`, and `docs/spec-readiness.md` — see [docs/design-review.md](docs/design-review.md).
- Research mapping (citation-only): prototype addresses Amara's and Tom's "give me a next step" and "ask something of me" complaints well; does not yet address whether the experience changes week to week (Tom/Amara's "static" complaint), Priya's deeper non-obvious pattern recognition, or whether repeated summaries actually bridge users to her ~3-month payoff.
- Lena's sharpest catch: caught the PM citing v3's bottom-nav fix (unvalidated, synthetic-session-only) to answer a question about v1/index.html (the only live-user-tested version) — surfaced a real process gap, not just a design note. Going forward: always name which prototype version and validation status is being referenced.
- Also surfaced a real sequencing conflict: Raj's spec-readiness.md language ("buildable... no new infra") reads like a design green light for the flat-week/cold-start states, but those states have never been designed or user-tested. Fixed: design validates non-happy-path states before engineering builds against them, not in parallel — to be communicated directly to Raj.
- Single highest-impact change identified: design and validate the flat-week, cold-start, and week-2-variety states before shipping — the happy-path-only prototype risks recreating the exact "nothing changed" failure mode the feature was built to fix, since most real weeks won't have a dramatic spending spike.

## Day 2 (cont.) — 2026-07-08 — QA pass on v3, spec/prototype drift found

- Ran a PM QA checklist against `prototype/v3.html` (edge cases + 10-point screen-by-screen pass) — see [docs/qa-checklist.md](docs/qa-checklist.md).
- Generated a full edge-case list across empty states, edge data conditions, multi-account scenarios, and permission states for QA to work from.
- **Two blocking findings from tracing v3's actual code against the agreed spec:** (1) the Savings Goals card still shows two goals (Emergency Fund + Credit Card Payoff), contradicting the single-goal-for-v1 decision locked in `docs/spec-readiness.md`; (2) the nudge card's copy still promises mid-week alerting ("we'll flag it then too"), which was explicitly cut to backlog per Raj's own recommendation in the same spec review — a real over-promising risk, the same failure mode flagged for competitors in `docs/decision-brief.md`.
- Third blocking finding: the transaction drill-down's listed totals ($141.95) plus its "+ 2 more orders, totaling $164.00" note is ambiguous and can be misread as contradicting the $164 headline — undermines the trust/verification purpose the drill-down was built for.
- One known (non-blocking) issue tracked: the nudge's progress bar visually caps at 100% width even though actual spend is ~2.7x the limit, understating the overage.
- Drafted a PR comment for Raj asking (as a question, not a demand) whether the mid-week alerting copy is a leftover from before the backlog decision, or a scope change worth knowing about.

## Day 2 (cont.) — 2026-07-08 — Metric analysis: retention data pulled and queried

- Retrieved the Nudge dataset (`Copy of Nudge Dataset.xlsx` — 5 tabs: Users, Sessions, Retention, Nudges, Weekly_Summary_Sends) after both provided Google Sheet links failed to resolve via connected Drive access; loaded into SQLite and ran SQL directly against it.
- Answered 4 core questions — see [data/metric-findings.md](data/metric-findings.md): 30-day retention declines cohort over cohort (32%→22%, weeks 1-4); week-1 goal-setters retain at ~1.7x non-setters (36.4% vs 21.8%), confirming Raj's original finding; week-5 treatment vs. control shows a real lift (day-7: 76% vs 46%, day-30: 36% vs 22%, n=50/arm); weekly summary open rate climbed across its first 4 sends (28%→56%) while control stayed flat (~4-6%), early evidence against novelty decay.
- Recommendation grounded in the data: run a larger/longer test before full rollout, not skip straight to 100% scale, given the single 50/50 cohort.

## Day 2 (cont.) — 2026-07-08 — Full metric diagnosis

- Ran a deeper diagnosis on the same dataset — see [data/metric-diagnosis.md](data/metric-diagnosis.md): a metric tree decomposing 30-day retention into activation (flat 90-97%, not the problem) vs. day-1→day-7 and day-7→day-30 stages (where the entire decline concentrates).
- **Mechanistic finding:** week-4 cohort avg sessions/user (3.69) and week-5 control (3.68) are statistically identical — control was on track to repeat the same decline. Week-5 treatment jumped to 5.28 avg sessions/user, nearly restoring week-1's original level (5.74) — the summary is measurably reversing the specific engagement-erosion pattern that explains the weeks-1-4 decline, not just generically "helping."
- Generated 4 ranked, data-scored hypotheses for why some treatment users still churned. Most notable: Hypothesis 4 (no savings goal explains churn) — the most intuitive story — is contradicted by the data (63.6% vs 64.3% churn, no real gap) and ranked lowest. Hypothesis 1 (platform-specific gap: iOS treatment churn -25pp vs. control, Android flat) ranked highest and recommended as the next-sprint test — strongest effect size, cheapest to confirm, and actionable even before full root-cause is known.

## Day 2 (cont.) — 2026-07-08 — Results memo for Marcus

- Wrote a one-page results memo for Marcus (situation/evidence/recommendation/ask/risk-of-waiting format, recommendation in the first sentence per his profile) — see [docs/recommendation-memo.md](docs/recommendation-memo.md).
- Recommendation: expand the test to a larger cohort before committing to full rollout — the day-30 lift (36% vs 22%) and the session-frequency mechanism are real, but currently rest on a single 50-vs-50 split.
- Simulated 3 hardest questions from Marcus (per his stakeholder profile): n=50 statistical confidence, a quantified cost-of-waiting number (his own previously-unanswered question — memo still doesn't fully close this gap, flagged honestly), and whether the platform gap (iOS-only benefit) means this is a company-wide win or a partial one given Android's flat result.

## Day 2 (cont.) — 2026-07-08 — Full experiment design, pilot pressure-tested

- Pressure-tested the pilot result and designed the full follow-up test — see [data/experiment-design.md](data/experiment-design.md).
- **Pilot is not statistically significant at 95% confidence** (p ≈ 0.12, z ≈ 1.54, 95% CI on the day-30 difference spans roughly −4pp to +32pp) — a real, honest finding that tempers the recommendation-memo's headline numbers.
- Full test sized at MDE=5pp, 80% power, 95% significance: ≈1,160 users/variant (≈2,320 total) — fits within the 8-week/85,000-WAU constraint at ≈7.3 weeks if enrollment is spread across 3 cohort weeks (recommended, to avoid the single-week fragility that limited the original pilot).
- Recommendation: run the full test before recommending scale — risk of waiting (~5-7 more weeks of cohorts on the old decline trajectory) vs. risk of scaling now (committing to an effect that may be smaller than observed, or null).
- Defined 3 weekly leading indicators to avoid waiting blind for the full 7.3 weeks: summary open-rate trend, avg sessions/user (the causal mechanism identified in the diagnosis), and day-7 retention gap as an early proxy for day-30.

## Day 2 (cont.) — 2026-07-08 — PRD for Raj and Lena

- Wrote a one-page PRD for the personalized weekly summary feature, written for Raj and Lena directly (plain declarative language, no leadership framing) — see [docs/prd.md](docs/prd.md).
- Synthesized from `research/interview-synthesis.md`, `research/nps-analysis.md`, `research/competitive-matrix.md`, and `docs/hypothesis.md` (corrected from the requested `research/hypothesis.md`, which doesn't exist).
- Incorporates decisions already locked with the team: single goal for v1, mid-week alerting cut to backlog, novel-insight ranking logic defined as biggest % deviation from personal average. Open questions section carries forward the still-unresolved threshold number, unbuilt fallback states, and variety-rule ownership from the spec review.

## Day 2 (cont.) — 2026-07-08 — PRD pressure-test / objection log

- Pressure-tested `docs/prd.md` against three simulated reviewers — see [docs/objection-log.md](docs/objection-log.md): skeptical Raj (execution/scope), skeptical Marcus (strategy/resourcing), and churned user Tom (does this actually solve his problem).
- Raj: pushed on "Open Questions" actually being unresolved blockers (threshold, fallback states, variety-rule ownership) that block sprint sizing, and no rollback plan for the nudge's write-back action.
- Marcus: pushed on whether this closes the retention gap or just delays the same churn, and — most critically — why a PRD is being scoped for full build when `data/experiment-design.md` already shows the pilot isn't statistically significant yet.
- Tom: pushed on "one insight, once a week" still leaving 6 empty days, and whether the tracked goal is really his choice or something generic the app picked.
- **Highest kill-risk identified: Marcus's resourcing objection.** Raj's and Tom's objections are solvable within an already-funded effort; Marcus's threatens whether the initiative gets resourced at all — if the PRD-vs-pilot-significance inconsistency surfaces in his review instead of being addressed upfront, the likely outcome is a 5-7+ week resourcing pause, not just a design note. Flagged to resolve before sharing the PRD with him.

## Day 2 (cont.) — 2026-07-08 — Weekly status skill upgraded to dual-audience

- Generated this week's status update in two forms from the same raw notes (interview synthesis/competitive matrix/prototype v1 shipped; PRD 70% and usability scheduling in progress; ranking-logic backfill blocker; Marcus out Thu-Fri) — a conversational team update for Raj/Lena and a terse 4-line leadership update for Marcus.
- Used the exercise to upgrade [skills/weekly-status.md](skills/weekly-status.md) from a single generic format into a reusable skill that produces both audience-specific outputs (team + leadership) from one set of notes every time it runs, rather than just the original Shipped/In Progress/Blockers/Next Week format.

## Day 2 (cont.) — 2026-07-08 — Quarterly review deck (structure + speaker notes)

- Designed a 6-slide quarterly-review narrative and full speaker notes for Marcus — see [docs/presentation.md](docs/presentation.md) (structure) and [docs/presentation-notes.md](docs/presentation-notes.md) (spoken notes).
- Structure: Problem (37% retention, down from 44%) → Why Now (converged evidence + real pilot data) → Proposal (what it is/isn't, scoped tightly) → Evidence (prototype, user quotes, pilot numbers) → Plan (full-test timeline, milestones, risks) → Ask (fund the test, not a rollout).
- Deliberately built in two fixes from the earlier PRD objection-log exercise: Slide 4 states outright that the pilot isn't statistically significant yet (p ≈ 0.12), and Slide 6's ask is explicitly framed as funding the properly-powered test, not a company-wide rollout — closing the exact resourcing objection flagged as highest kill-risk in [docs/objection-log.md](docs/objection-log.md).

## Day 2 (cont.) — 2026-07-08 — Workspace audit and CLAUDE.md rewrite

- Audited the full workspace (~25 files across 5 folders) against what's actually in `CLAUDE.md` — see [workspace-audit.md](workspace-audit.md).
- **Biggest finding:** CLAUDE.md was frozen at Day-1 discovery state ("open decision not yet resolved," direction "early, being explored") despite everything shipped since — Marcus's approval, 3 prototype versions and multiple test rounds, a real pilot, the full-test design, a pressure-tested PRD, and the quarterly deck. Rewrote CLAUDE.md to reflect current status, added a "Where Things Live" file map, and pointed to the stakeholder profiles and prototype version/known-bugs status.
- Other findings: `CLAUDE.md`, `project.md`, and `strategy.md` have drifted out of sync (`project.md` still says "Current Phase: Discovery") — recommended treating CLAUDE.md as the now-authoritative summary rather than editing all three; `PRD Skeleton.md` at root is superseded by `docs/prd.md` and should be archived/moved; the source dataset (`Copy of Nudge Dataset.xlsx`) lives outside the workspace in `~/Downloads`, not alongside its derived analysis in `data/`; `prototype/v3.html` has 3 known unfixed bugs from the QA pass whose status wasn't visible anywhere before this update.

## Day 2 (cont.) — 2026-07-08 — Three one-command PM workflows

- Designed and saved three self-contained, single-paste workflow skills for the most repetitive PM tasks — see [skills/friday-status.md](skills/friday-status.md), [skills/research-synthesis.md](skills/research-synthesis.md), [skills/competitive-pulse.md](skills/competitive-pulse.md).
- **Friday status:** trigger "Run my Friday status update" — reads `change_log.md` directly to classify shipped/in-progress/blocked itself (no notes to paste), outputs team + leadership updates, saves to `status-updates/YYYY-MM-DD.md`.
- **Research synthesis:** trigger "Run my weekly research synthesis" — pulls new material from a `research/inbox/` drop folder, synthesizes using the same method as the existing NPS/interview work, and explicitly flags whether findings confirm, contradict, or are new vs. existing research. Saves to `research/synthesis/YYYY-MM-DD.md`.
- **Competitive pulse:** trigger "Run my competitive pulse check" — already knows the 5 tracked competitors from `competitive-matrix.md`, searches for recent moves, flags anything touching the two identified white-space gaps, and distinguishes "no change" from "search was inconclusive." Saves to `research/competitive-pulse/YYYY-MM-DD.md`.
- All three are designed to run from a single pasted trigger phrase with no follow-up input — each gathers its own inputs from the workspace or web rather than requiring the user to supply raw material first.

## Day 2 (cont.) — 2026-07-08 — Monday retention digest agent (built and verified)

- Designed a Monday-morning agent spec — see [agents/monday-retention.md](agents/monday-retention.md): checks 30-day retention, avg sessions/user, and push/nudge open rate against last week's baseline, identifies the biggest mover, and posts a plain-English Slack digest (headline number, one signal to watch, one suggested action).
- Actually built and ran the Python script against the real dataset (not just described it) to verify real output before writing up the spec.
- **New finding surfaced by the verification run:** push/nudge open rate is the biggest relative mover between the two most recent cohorts (-34.3%), larger than the retention drop (-12.0%) or the sessions-per-user drop (-17.6%) — not previously called out in `data/metric-diagnosis.md`, worth a follow-up look independent of the weekly-summary-specific findings.
- Documented as spec + manually-verified script only, not yet live — "Path to Production" section spells out the 3 changes needed (real rolling-window data source, cron/n8n scheduler, Slack webhook delivery) with no other logic changes required.

## Day 2 (cont.) — 2026-07-08 — Capstone review and 3 fixes implemented

- Ran a full capstone review of the workspace (inventory, per-artifact confidence scoring, proposed fixes, portable prompt for reuse on another product) — see [docs/capstone-session.md](docs/capstone-session.md).
- Biggest finding: data provenance for `Copy of Nudge Dataset.xlsx` was never confirmed as real production data vs. an illustrative dataset — every pilot-derived artifact inherits this uncertainty.
- Implemented all 3 proposed fixes:
  1. **Fixed the 3 QA bugs in `prototype/v3.html`** — reverted to single savings goal (v1 scope), removed the mid-week alert copy that promised a backlogged feature, fixed the ambiguous transaction-drill-down total.
  2. **Cleaned up root-level staleness** — marked `project.md`'s phase section stale inline pointing to CLAUDE.md; moved `PRD Skeleton.md` to `archive/` with a superseded notice.
  3. **Added data-provenance disclaimers** to every file that depends on the pilot dataset: `data/metric-findings.md`, `metric-diagnosis.md`, `experiment-design.md`, `docs/recommendation-memo.md`, `docs/presentation.md`, `docs/presentation-notes.md`.
- Updated `CLAUDE.md` to reflect all fixes as resolved and point to `archive/` instead of the old `PRD Skeleton.md` location.

## Day 2 (cont.) — 2026-07-08 — Onboarding materials for a new Claude Code teammate

- Wrote a one-page onboarding guide and a 15-minute live demo script for a teammate new to Claude Code — see [docs/onboarding-guide.md](docs/onboarding-guide.md) and [docs/onboarding-demo-script.md](docs/onboarding-demo-script.md).
- Guide covers: what Claude Code is (2 sentences, no jargon), what they'll build using the portable prompt from `docs/capstone-session.md`, the 3 core habits (keep CLAUDE.md current, interview before building, save everything), and the single most common mistake (treating it as a one-shot answer machine instead of steering it).
- Demo script is minute-by-minute (0-2 open the folder and read CLAUDE.md aloud, 2-7 run the AI interview + plan review, 7-12 run the Friday-status skill then hand over the keyboard, 12-15 they paste the portable prompt into their own new folder) and ends with them mid-interview in their own workspace, not watching the presenter drive.
- Saved the original `#product-engage` Slack thread (previously only pasted text, not a file) to [research/nudge-slack-thread.md](research/nudge-slack-thread.md) so the demo script has a real file to point to instead of relying on external course materials.
