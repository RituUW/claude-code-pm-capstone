> **⚠️ Data provenance unconfirmed** — see `data/metric-findings.md`. The pilot numbers cited in this memo have not been verified as real production data. Confirm before this goes to Marcus as-is.

**Recommendation up front: run one larger confirmatory test of the weekly summary before committing to full rollout — the early data is strong, but the sample is too small to bet the Q3 roadmap on yet.**

# Weekly Summary — Results & Recommendation

## Situation

We shipped the personalized weekly summary (top insight, one actionable nudge, savings goal progress) as a randomized test to week-5 signups only — 50 users on the new experience, 50 on control. We were testing the core hypothesis from the decision brief you approved: that giving users a reason to come back weekly, not just at signup, would repair the day-7/day-30 retention drop-off.

## Evidence

- **Retention moved, and moved specifically in the number you asked about.** Day-30 retention: 36% (treatment) vs. 22% (control) — a 14-point lift. Day-7: 76% vs. 46%.
- **We found the actual mechanism, not just a correlation.** Week-4 cohort and week-5 control both averaged ~3.7 sessions/user — control was on track to repeat the same decline we've been watching since week 1. Treatment users averaged 5.28 sessions — nearly back to week-1's original level (5.74). The summary is measurably reversing the exact erosion pattern driving the 44%→37% decline, not just generally "helping."
- **The lift isn't a one-time novelty spike.** Open rate on the summary itself climbed across its first 4 weekly sends (28%→56%) while control stayed flat at 4-6% — engagement is building, not decaying, which was the specific risk we flagged from competitor research (Cleo's engagement wearing off in about a week).

## Recommendation

Expand the test to a larger cohort (or run it 1-2 more cohort weeks) before committing full engineering scope to a company-wide rollout — the effect is real and large, but it's currently a single 50-vs-50 split.

## Ask

Approve resourcing for an expanded test — I'd like your sign-off by our next 1:1 so it can start with the next cohort week rather than losing another cycle. In parallel, I want to keep the already-scoped engineering work (per the spec review with Raj) moving on the parts we're not waiting on data for, so we don't stall momentum while the bigger test runs.

## Risk If We Wait

Every cohort week we don't act, we're adding another 100 users to the same decline trajectory that took week-4's day-30 retention down to 22% — the fix is sitting validated and unscaled while the baseline keeps eroding underneath it.

---

## Simulated Q&A — Hardest Questions from Marcus

*Roleplayed using `stakeholders/marcus.md` — skeptical, pushes for business outcome and pressure-tested confidence, not just an interesting result.*

**1. "n=50 per variant — how do I know this isn't noise?"**
Give me the actual confidence interval, not just the percentage. A 14-point swing sounds great on 50 people, but I've seen enough small-sample wins evaporate at scale that I'm not moving Q3 roadmap on a single cohort. If I had someone independently check this, would it hold up?

**2. "What is this actually costing us to wait — in real numbers, not urgency language?"**
You've told me before that waiting has a cost, and I've asked you to quantify it and you haven't yet. You now have the exact decline curve — 5.74 sessions down to 3.69, day-30 from 32% to 22%. So tell me: if I say "come back in one more cohort week with a bigger sample," what does that actually cost us in users or retained accounts, not just "erosion continues"? Put a number on the four-week ask, not just the four-week ask itself.

**3. "If Android users aren't benefiting, is this actually a company-wide win or a half-a-win?"**
You found a 25-point swing for iOS and basically nothing for Android. Before I greenlight scaling this, I need to know what share of our user base that Android gap actually represents — if it's a third of MAU, I'm not approving a roadmap bet on a feature that structurally only works for two-thirds of our users. Is this a "ship now, fix Android later" story or a "we don't actually know if this generalizes" story? Those are very different asks.
