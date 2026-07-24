# Quarterly Review — Speaker Notes

> **⚠️ Data provenance unconfirmed** — see `data/metric-findings.md`. Confirm the pilot data is real production data before delivering these notes to Marcus as-is.

## Slide 1 — The Problem

This slide shows one number: our 30-day retention has dropped from 44% to 37% over the last two quarters. That's not a rounding error — it's a real and worsening trend, and it's the reason this whole initiative exists. The insight underneath the number is what matters most: users clearly get value on day one, the spending breakdown genuinely lands for them, but the app goes quiet after that first week, and they stop coming back before they ever get far enough in to discover anything deeper. Before we move on, I want you to walk away from this slide believing that this is a retention-timing problem, not an acquisition or a core-value problem — because that distinction is what shapes everything I'm about to propose.

## Slide 2 — Why Now

This slide is about what's actually changed since we last talked about this. We started the quarter with a hunch, and we're ending it with four independent sources of evidence — user interviews, NPS feedback, competitive research, and now a real randomized pilot — all converging on the same explanation. What we learned that we didn't know before is genuinely specific: we can now point to the exact mechanism behind the decline, average sessions per user eroding from 5.74 down to 3.69 cohort over cohort, and we have a candidate fix that's already been tested against that exact number, even if only at small scale so far. I want you to leave this slide with confidence that we're not chasing a vague feeling anymore — we found the actual lever.

## Slide 3 — The Proposal

Here's what we're actually proposing to build: a personalized weekly summary that gives users one non-obvious insight, one specific action to take, and visible progress on their savings goal, every week — and it picks up exactly where our weekly email already leaves off, since that email already gets a 22% open rate but currently dead-ends the moment someone clicks into the app. Just as important is what this isn't: we are deliberately not building multi-goal support, not building mid-week alerts, and not trying to replicate the kind of deep, months-long personalization that keeps our power users around — that's a real opportunity, but it's not this quarter's bet, and I don't want scope creep to blur that line. What I want you to take away here is that this is a tightly scoped, deliberately modest first step, not an attempt to solve retention in one shot.

## Slide 4 — Evidence

This slide is doing three things at once: showing you the prototype held up in real usability testing, letting the users speak for themselves, and being honest about what the data does and doesn't yet prove. The quotes aren't cherry-picked for flattery — Tom telling us he left for YNAB because it "asked something of him," and Amara telling us she was just waiting for Nudge to tell her the next step, are exactly the gaps this feature is built to close. The pilot numbers are genuinely encouraging — a 14-point lift in 30-day retention and a mechanism we can actually explain — but I want to be direct with you: at 50 users per group, this result is not yet statistically significant, and I'd rather tell you that plainly now than have you find out later that I oversold a coin flip. What I want you to walk away with is real optimism paired with real discipline about what we still don't know.

## Slide 5 — The Plan

This is the roadmap for turning "promising" into "proven." We've sized a properly powered follow-up test — about 1,160 users per group, enrolled over three weeks to avoid the single-week fragility our pilot had, wrapping up in roughly seven and a half weeks, which comfortably fits inside an eight-week window. We're not going to wait blind for seven weeks to find out if something's wrong — we've picked three leading indicators, especially that sessions-per-user number, that we'll watch weekly so we catch problems early. I do want to flag two real risks honestly: Raj surfaced a data backfill for the ranking logic that's still unestimated, and the pilot showed a real gap where iOS users benefited a lot and Android users barely moved at all, which we need to understand before we assume this works for everyone equally. Leaving this slide, I want you confident that we have a specific, time-boxed plan, not an open-ended "let's see."

## Slide 6 — The Ask

Here's exactly what I need from you: approval to resource the full, properly powered test — not a company-wide rollout, just the test itself — with a decision by our next 1:1 so we can start enrollment with the next cohort week instead of losing another cycle waiting. I also want your support to keep the engineering work that's already scoped moving in parallel, the pieces that don't depend on this test's outcome, so we're not sitting idle for two months on a single gate. This is a small, bounded ask relative to the size of the retention problem it's aimed at, and it gets us a real, trustworthy answer within this quarter rather than another quarter of debate. That's what I'm asking you to approve today.
