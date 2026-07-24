# Nudge Engage v2 — Weekly Summary Prototype

Open `index.html` directly in a browser — no build step, no dependencies.

## PM Brief

**User:** 28-year-old who connected their Chase account 2 weeks ago and has not opened the app since. Set a savings goal during onboarding.

**Job to be done:** Understand where their money went this week and take one action.

**Feature:** Personalized weekly money summary — top spending insight, one contextual nudge, savings goal progress.

**Constraint:** Use data Nudge already has — no new integrations.

**Grounding research:** `research/interview-synthesis.md`, `research/nps-analysis.md`, `research/competitive-matrix.md`, `research/competitive-reddit.md`, `docs/decision-brief.md`.

## Key Decisions Made During the Interview

1. **Purpose: user testing.** This prototype is meant to go in front of real users to validate the concept, not just pitch stakeholders — so it needed to feel real and respond to interaction rather than being a static mock.
2. **Persona has a savings goal set (not goal-less).** This lets the prototype directly test the NPS finding that users who set a goal feel Nudge "never mentions it again" — the goal-progress card here proactively closes that loop.
3. **The nudge requires an explicit action, not passive display.** The research surfaced a real tension: Priya (retained) responded to passively surfaced insights, while Tom (churned) explicitly wanted the app to "ask something" of him. Since this persona is already lapsed — closer to Tom's profile — the nudge includes a real action button ("Set a $60/week limit") plus a dismiss option, rather than just showing text.
4. **Clicking the nudge action simulates a specific state change, not a generic confirmation.** Applying the limit visibly updates a progress bar and shows a concrete downstream projection tied to the savings goal ("could get you there ~6 weeks sooner") — testing whether specificity, not just acknowledgment, is what makes the nudge feel credible.
5. **Entry point: the weekly summary email, not a push notification.** Interview/NPS research specifically flagged that the email works (22% open rate) but the in-app experience doesn't continue its story after click-through. The prototype starts on a mock email and the CTA leads into the app screen, directly testing whether that disconnect is fixed.
6. **Visual style: clean, modern fintech.** Soft neutral background, a single accent color, minimal chrome — deliberately unbranded/generic so feedback is about the concept, not Nudge's existing visual identity.

## What's Mocked vs. Real

- All spending/goal data is hardcoded (Jordan, $164 food delivery, $1,000 Emergency Fund goal) — there is no backend or ranking logic. This is meant to test reaction to the *concept*, not the real ranking engine Raj would need to build.
- The "Set a $60/week limit" action is a simulated state change (bar fill, projection text) — it doesn't persist or connect to real budgeting logic.
- Use the "↺ Replay prototype" link at the bottom of the app screen to reset and re-run the flow with a new test participant.

## Files

- `index.html` — the full prototype (email screen → app screen, self-contained HTML/CSS/JS)
- `README.md` — this file
