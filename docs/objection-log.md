# PRD Objection Log — Personalized Weekly Summary

Pressure-testing `docs/prd.md` against three reviewers, in character.

---

## 1. Skeptical Raj (Engineering)

*Using `stakeholders/raj.md` — pushes back on underspecified requirements and scope touching data without a rollback plan; needs acceptance criteria and "what does done look like" before saying yes.*

**Q1:** The PRD lists the deviation threshold, the fallback states, and who owns the variety rule as "Open Questions" — like they're fine to leave loose. They're not open questions, they're blockers. I can't size a sprint around "TBD." Who owns deciding each of these, and by when — before kickoff, or am I supposed to guess and build twice?

**Q2:** The nudge says "cap category X at $Y/week" — does tapping that actually write a real limit somewhere, or is it just a UI state for v1? If it persists, what's the rollback if the ranking logic recommends a bad limit to a real user? I don't see a rollback plan anywhere in this doc, and that's exactly the kind of thing I flag before touching anything that writes back to user data.

---

## 2. Skeptical Marcus (Strategy & Resourcing)

*Using `stakeholders/marcus.md` — pushes back on recommendations without a clear ask, data not tied to business outcome; needs a pressure-tested case before committing resourcing.*

**Q1:** This PRD's own Non-Goals say we're explicitly *not* building the mechanism that actually retains long-tenured users — so what's the plan after this ships? Are we confident this closes the retention gap, or does it just push the same churn out a few more weeks before it happens anyway?

**Q2:** We already know from the experiment design that the pilot result isn't statistically significant yet — so why is a full PRD being scoped for engineering before the properly powered test has even run? If I resource a sprint against this now and the bigger test comes back null, what did we just spend?

---

## 3. Churned User — Tom

*Using the interview synthesis directly — skeptical of budgeting apps generally, doesn't want to be lectured, left because the app "asked nothing of him" and felt static.*

**Q1:** One insight, one nudge, once a week — that's still just one thing to look at, once. What happens the other six days? If I open the app on a Wednesday, is there actually something there, or is this just a fancier version of "come back Monday"? Because "come back later" is exactly why I left.

**Q2:** You're telling me you'll show my savings goal progress every week — but what if the goal you're tracking isn't even the thing I actually care about? How do I know this is really about something I chose, and not just whatever the app decided to track for me?

---

## Which Objection Would Kill the Initiative

**Marcus's second question is the one to address before anything else.**

Raj's objections are about execution quality — real, and they'd slow a sprint down or force a rebuild, but they're solvable within an already-funded effort. Tom's objections are about product depth — important for whether this actually works long-term, but they don't stop the project from being built and tested.

Marcus's question is different: it's about whether the initiative gets resourced *at all*, right now. If he reads this PRD, notices it's scoped for a full build, and connects that to the fact that the pilot isn't statistically significant yet (per `data/experiment-design.md`), the natural response is to pause engineering investment until the properly powered test concludes — which could stall the whole initiative for 5-7+ weeks regardless of how good the PRD itself is. That's not a design fix or a scope clarification; it's a funding decision, and it needs to be answered *before* this PRD is shared with him, not discovered in the review.
