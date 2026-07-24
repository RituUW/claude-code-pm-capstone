# Design Review Prep — Weekly Summary Prototype

Prepared for: review with Lena
Sources: `prototype/index.html`, `research/interview-synthesis.md`, `docs/spec-readiness.md`, `stakeholders/lena.md`

## 1. Research × Prototype — What's Addressed, What Isn't

**Addressed well (cited):**
- **A concrete next step after the insight.** Amara: *"I have been waiting for Nudge to tell me the next step."* Tom: *"I kept waiting for it to give me something to act on and it never really did."* → The "One Thing To Try" nudge card gives a specific, named action directly after the insight.
- **The app asking something of the user, not just displaying data.** Tom: *"I switched to YNAB because at least that feels like it is asking something of me."* → The nudge requires an explicit action (set limit / dismiss), not passive viewing.

**Not yet addressed (cited):**
- **Whether the experience actually changes over time.** Tom: *"The app just showed me the same dashboard every time."* Amara: *"I check it occasionally but nothing has changed since the first day."* → The prototype is a single fixed instance; no week-2 state has been designed or tested.
- **Deep, non-obvious pattern recognition vs. a single-week threshold.** Priya: *"It started surfacing patterns I had not consciously registered — like that I always overspend in the last week of the month."* → Current insight logic (biggest % deviation from average) is simpler than the longer-horizon pattern that actually created her stickiness.
- **Bridging the "aha moment" to sustained value.** Priya: *"It took about three months to get there and I think most people give up before that."* Amara: *"The spending breakdown was shocking in a useful way... but now I do not really know what to do with that information."* → Untested whether repeated summaries bridge this gap or just delay the same dead-end.

## 2. Design Review Exchange — Summary

Simulated as Lena (per `stakeholders/lena.md`), three hardest questions and how each resolved:

| Lena's question | Grounded in | Resolution / commitment |
|---|---|---|
| "Walk me through week 2 — what's different?" | Tom: *"same dashboard every time"* | PM owns defining the actual repeat/variety rule with Lena, not defaulting it to engineering. A real week-2 mock is committed as the next artifact — not a description of one. |
| "Why would someone open this on a Tuesday?" | Lena's own previously-unanswered question | Caught a real inconsistency: `index.html` (v1, live-user-tested) doesn't solve this; v3's bottom-nav fix does, but v3 is unvalidated (synthetic persona session only). Going forward: always name which version and validation status is being referenced. |
| "Where's the evidence the flat-week state won't feel like 'nothing changed' again?" | Tom + Amara, static-experience complaints | No evidence exists yet — flat-week and cold-start states are spec'd (`docs/spec-readiness.md`) but never designed or tested. Sequencing fixed: design validates these states with users *before* engineering builds against them, not in parallel. Raj's "buildable" was a feasibility statement, not a design sign-off — will be clarified with him directly. |

## 3. Single Highest-Impact Change for Week-1 Retention

**Design and validate the non-happy-path states (flat week, cold start, and week-2 variety) before building any of them — not just the strong-insight state already prototyped.**

Why: the single most repeated failure mode across all research (Tom, Amara, NPS) is the app feeling static and unchanging. The prototype currently only demonstrates the one case — a dramatic spending spike — that's least likely to be the common case in real usage. Most weeks for most users will be flat or unremarkable. If those states ship un-designed and un-tested, the feature risks *recreating the exact problem it was built to solve*, just inside a shinier wrapper. This is a bigger risk to week-1 retention than any visual or copy polish on the happy-path screen already reviewed.

## 4. Product Decision vs. What Lena Should Own

**Product decisions (PM owns):**
- The repeat/variety policy — what counts as a strong-enough signal to resurface a category two weeks running (defined collaboratively with Lena, but the tradeoff call is product's).
- Sequencing and scope: design-validates-before-build gating, what's cut to backlog (e.g., mid-week alerting, multi-goal), single-goal decision for v1.
- Success metrics and instrumentation requirements (open/action events, cohorting).
- Whether "positive reframe" (e.g., promoting goal progress during a flat week) is the right *strategic* approach to pursue at all, prior to testing.

**What Lena should own:**
- Actual copy, layout, and visual design for all three states — happy path, flat week, cold start — not just the one already built.
- Whether the positive-reframe approach, once tested, actually reads as reassuring vs. hollow to real users.
- The wayfinding/entry-point pattern (context strip vs. bottom nav vs. something else) and validating it solves the Tuesday-access problem for real.
- Visual hierarchy across cards (e.g., does "More Places To Save" compete with the primary nudge) and tone/voice calibration on nudge copy.
- Sign-off gate: nothing in the non-happy-path states goes to engineering as final until she's validated it with users, per the sequencing fix agreed in this review.
