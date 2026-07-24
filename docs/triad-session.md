# Triad Working Session — Engage v2 Prototype Review

Attendees: Raj (Eng), Lena (Design), PM
Duration: 30 minutes
Prep: `prototype/v3.html` (open directly in a browser), `docs/hypothesis.md`

## Session Agenda

### 1. Context recap (5 min)
- 30-day retention: 44% → 37%. Root cause confirmed across 4 independent sources (interviews, NPS, competitive matrix, Reddit sentiment) — see `docs/decision-brief.md`.
- Hypothesis: a personalized weekly in-app summary gives users a concrete, recurring reason to return and act — see `docs/hypothesis.md`.
- This session is about the *prototype*, not re-litigating the problem — assume the direction is set, focus on what to build and how.

### 2. Prototype walkthrough (10 min)
Live click-through of `prototype/v3.html`, narrating what changed and why at each version:
- **v1:** Core concept — email → in-app summary with top insight, one explicit-action nudge, single savings goal. Validated in live usability testing: both participants understood it unprompted and said they'd act on it.
- **v2:** Added wayfinding context (the #1 friction point from round 1 — neither user could say how they'd arrive at the screen) and a "More Places To Save" category breakdown.
- **v3:** Addressed persona-interview findings — transaction drill-down (trust/verification), on-demand access via bottom nav (not just email-triggered), multi-goal support, mid-week proactive framing, and softened nudge tone.
- Flag explicitly: v3's changes are **not yet validated with live users** — they came from a synthetic interview session, not real participants.

### 3. Discussion (10 min)

**Questions for Raj (feasibility):**
- Is the "novel insight" ranking logic (surfacing non-obvious patterns, not confirming known ones) buildable with existing data sources, or does it need new signals we don't have?
- Multi-goal support (shown in v3) is new scope beyond the original PM brief — does our data model support multiple concurrent goals today, or is this a bigger lift than it looks?
- Transaction drill-down — is per-transaction detail readily queryable for real-time display, or would this require new indexing/infra work?
- Mid-week proactive alerting (flagged in v3 as a UI note, not a built mechanism) — what would it actually take to trigger a mid-week check versus our current weekly-batch cadence?
- Rough sizing: what's realistically buildable for a first shippable version vs. what should sit in a backlog?

**Questions for Lena (design):**
- Does v3's added density (two goals, expandable transactions, bottom nav) still feel like "one clear next step," or has it drifted from the original one-thing-to-try simplicity?
- Tone check on the nudge copy — does "Set a $60/week limit for food delivery?" read as neutral, or still lecturing? Does it match Nudge's voice?
- Does "More Places To Save" visually compete with the primary nudge for attention, or does the hierarchy hold?
- Is the wayfinding fix (context strip + bottom nav) the right pattern, or is there a cleaner way to solve "how did I get here" within our existing IA?

### 4. Decisions to walk out with (5 min)
- [ ] Which elements are in scope for a first shippable version vs. backlog (candidates to explicitly call: multi-goal support, transaction drill-down, mid-week alerting)
- [ ] Whether we run a live usability round on v3 before committing to build, or move straight to spec
- [ ] Owner and rough timeline for the ranking logic spec (Raj)
- [ ] Owner and rough timeline for a real design pass, if v3's direction is approved (Lena)
- [ ] Any hard blockers or unknowns that need answering before next steps can be scoped

---

# Post-Session Alignment Doc (Template)

*Fill out during/immediately after the session and share with the triad + Marcus.*

## Attendees & Date

-

## Context

*One or two sentences — link back to `docs/decision-brief.md` and `docs/hypothesis.md`, don't restate the whole problem.*

## What We Reviewed

*Prototype version(s) shown, what was in/out of scope for this review.*

## Decisions Made

| Decision | Rationale | Owner |
|---|---|---|
| | | |

## Open Questions / Parking Lot

*Anything raised but not resolved — carry forward, don't let it silently drop.*

-

## Action Items

| Action | Owner | Due |
|---|---|---|
| | | |

## Next Session / Checkpoint

-

---

# Slack Invite Message

Hey Raj, Lena 👋 — can I grab 30 min this week to walk through the Engage v2 prototype together?

Quick context: I've run it through a couple rounds of testing already (live usability sessions + a deeper persona-based review), and it's at a point where I want your eyes on it before we scope real build work.

**What we'll cover:**
- Quick recap of the retention problem + hypothesis (2 min, you've seen the decision brief)
- Live walkthrough of the prototype and what changed across versions and why
- Raj — I've got specific feasibility questions (ranking logic, multi-goal support, transaction-level data, mid-week alerting)
- Lena — I want your read on density/tone/hierarchy now that it's grown a few features since v1
- Walk out with: what's in scope for a first build vs. backlog, and whether we need another live test round first

**Before we meet:** if you get a sec, open `prototype/v3.html` in a browser — no setup needed, just click through it. Full agenda is in `docs/triad-session.md` if you want the details ahead of time.

Let me know what time works — trying to lock this in before it goes stale on my plate 🙂
