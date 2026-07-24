# Nudge Weekly Summary — Full Diagnosis

> **⚠️ Data provenance unconfirmed** — see `data/metric-findings.md`. Same caveat applies to every number below.

Builds on `data/metric-findings.md`. All numbers below come from additional queries against the same dataset (`Copy of Nudge Dataset.xlsx`), not estimates.

---

## 1. Metric Tree — What Actually Moves 30-Day Retention

```
30-Day Retention Rate
(retained_day_30 / total_signups)
│
├── Day-1 Retention (Activation)
│     Did the user come back at all after signup?
│     Drivers: acquisition_channel, platform, onboarding/first-session experience
│     → Data: flat at 90-97% across cohort weeks 1-4. NOT where the problem lives.
│
├── Day-1 → Day-7 Retention (Early Habit Formation)
│     Did the user keep opening the app in the first week?
│     Drivers: week-1 goal-setting, session frequency in week 1,
│               general nudge engagement (goal_reminder, weekly_tip, etc.),
│               weekly summary open (send 1), platform
│     → Data: THIS is where the decline concentrates (see Q2 below).
│
├── Day-7 → Day-30 Retention (Sustained Engagement)
│     Did the user keep coming back through the full month?
│     Drivers: continued weekly summary opens (sends 2-4), goal progress made,
│               session frequency trend, consistency of engagement (not just "ever engaged")
│     → Data: compounds the day-7 decline; average sessions/user erodes in lockstep.
│
└── Moderators (cut across all stages, not a separate funnel step)
      - Platform (iOS vs. Android vs. web) — changes how much any lever above actually works
      - Acquisition channel — paid-channel users retain modestly worse overall (21.3% vs. 29.2% organic)
      - Whether a savings goal exists — strong overall lever (Q2), though see Hypothesis 4 for a nuance
```

**Key takeaway from the tree:** activation isn't the leak. The leak is entirely in the two "keep coming back" stages — which is exactly the stage the weekly summary is designed to intervene on.

---

## 2. What Caused the Decline in Weeks 1-4 (Specific, Not Generic)

**The specific, data-confirmed story:**

| Cohort Week | Day-1 % | Day-7 % | Day-30 % | Avg. Sessions/User |
|---|---|---|---|---|
| 1 | 90.0% | 60.0% | 32.0% | 5.74 |
| 2 | 90.0% | 53.0% | 25.0% | 5.08 |
| 3 | 97.0% | 48.0% | 25.0% | 4.48 |
| 4 | 92.0% | 44.0% | 22.0% | 3.69 |

- **Day-1 activation is not the problem** — it's flat and high (90-97%) across every cohort. Whatever's failing isn't the initial signup/connect-account experience.
- **The decline is concentrated and monotonic in day-7 retention** — 60% → 53% → 48% → 44%, a clean, steady erosion, and day-30 tracks it down (32% → 22%).
- **Average sessions per user drops in exact lockstep** — 5.74 → 5.08 → 4.48 → 3.69. Users aren't failing to sign up or failing to connect an account; they're simply opening the app less and less as each successive cohort's first month plays out.
- **This is a between-cohort decline, not just normal within-cohort tenure decay.** Later-signing-up cohorts do worse at the *same* tenure milestones than earlier cohorts — week 4's day-7 rate (44%) is meaningfully below week 1's day-7 rate (60%) even though both are being measured 7 days after their own signup. That rules out "retention naturally decays with tenure" as a full explanation; something about the experience (or the users arriving) was getting worse over calendar time, independent of how long any individual user had been around.
- **Ruled out as the explanation:** acquisition-channel mix doesn't shift cleanly across weeks 1-4 (paid share: 31%, 42%, 32%, 31% — no consistent trend), and general nudge open rates are noisy, not trending (12.1%, 11.7%, 14.3%, 9.4%) — neither tracks the smooth decline the way session frequency does.
- **What the data can't tell us:** *why* cohort quality/engagement eroded week over week before the experiment started — that could be a product change, a seasonal effect, or a marketing-quality drift not captured in this dataset. What's confirmed is *where* it happens (post-activation, ongoing engagement) and that it's measurable, steady, and real — a quantified version of the "app stops feeling relevant after week 1" finding from the interviews, not just a qualitative impression.

---

## 3. What Week 5 Treatment vs. Control Tells Us the Summary Actually Fixed

The clearest evidence is the session-frequency comparison, because it connects directly back to the mechanism identified in Q2:

| Group | Avg. Sessions/User |
|---|---|
| Week 1 cohort (pre-experiment baseline) | 5.74 |
| Week 4 cohort (bottom of the decline) | 3.69 |
| **Week 5 Control** | **3.68** |
| **Week 5 Treatment (summary_v1)** | **5.28** |

**This is the mechanism, stated precisely:** Week 5's control group continued the exact decline trajectory already in motion — 3.68 avg sessions/user, statistically indistinguishable from week 4's 3.69. Left alone, this cohort was on track for the same day-7/day-30 numbers as week 4. The treatment group, by contrast, saw avg sessions/user jump to 5.28 — nearly back to the *week 1* cohort's original level (5.74), reversing the erosion rather than just slowing it.

**In plain terms: the weekly summary didn't invent a new kind of value — it restored the session frequency the product used to generate in week 1, and that restored frequency is what's driving the day-7 (+30pp) and day-30 (+14pp) retention lift already quantified in the findings doc.** It directly counters the specific, measured failure mode from Q2 (declining sessions/user), not a vaguer "engagement" story.

---

## 4. Four Ranked Hypotheses for Why Some Treatment Users Still Churned

### Hypothesis 1 — Platform-specific experience gap (RANK 1)

**Testable prediction:** "Android users who received the weekly summary churn at a rate similar to Android control users, while iOS treatment users churn notably less than iOS control users — implying the summary's retention mechanism doesn't translate well to Android."

**Data support:**

| Platform | Control Churn % | Treatment Churn % | Change |
|---|---|---|---|
| iOS | 77.3% | 52.2% | **-25.1pp** |
| Android | 70.0% | 72.7% | +2.7pp |
| Web | 100.0% | 80.0% | -20pp (n=5-8, too small to trust) |

**Likelihood rank:** 1 (strongest, most specific, most actionable signal in the data)
**Confidence: 7/10** — this is a real, sizeable directional gap in randomized treatment-vs-control data, not just a correlation within treatment. But per-cell sample sizes (n=22-23 for iOS/Android per arm) are still small enough that this needs confirmation before being treated as settled.
**Would confirm it:** Android treatment users showing measurably lower weekly-summary open rates or shallower engagement (fewer screens visited, shorter open_time_seconds) than iOS treatment users — evidence of a real delivery/UX friction point, not just a correlated outcome.
**Would rule it out:** If Android and iOS treatment users show statistically similar open rates and engagement depth, but churn still differs — the platform effect is more likely a confound (e.g., different user quality by acquisition channel that happens to skew by platform) than a real product gap.

### Hypothesis 2 — Partial/inconsistent engagement predicts churn more than total non-engagement (RANK 2)

**Testable prediction:** "Treatment users who open only 1-2 of their first 4 weekly summaries churn at a higher rate than users who open 3-4, indicating that partial engagement — trying it, not sticking with it — is a stronger churn signal than never opening it at all."

**Data support:**

| Opens (of 4 sends) | n | Churn % |
|---|---|---|
| 0 | 2 | 50.0% (too small to trust) |
| 1-2 | 36 | 69.4% |
| 3-4 | 12 | 50.0% |

**Likelihood rank:** 2
**Confidence: 5/10** — the 1-2 vs. 3-4 comparison is real and based on a reasonable sample (36 vs. 12), but causality is ambiguous: users who are already disengaging for unrelated reasons would naturally also open fewer summaries (reverse causation), not necessarily the other way around.
**Would confirm it:** Session data showing users in the "1-2 opens" bucket already had declining session frequency *before* they stopped opening summaries — supporting "early warning signal" framing regardless of strict causality, which would still be useful for an intervention (e.g., a re-engagement trigger after one missed open).
**Would rule it out:** If users who stop opening after 1-2 sends show no difference in other engagement (session frequency, screens visited) compared to consistent openers right up until their last active session — the "opens" pattern would just be noise correlated with, not predictive of, churn.

### Hypothesis 3 — Taking the suggested action isn't what drives retention; awareness alone may be enough (RANK 3)

**Testable prediction:** "Treatment users who take the nudge's suggested action (e.g., set a spending limit) do not retain better than treatment users who see the insight but decline the action — the retention value comes from awareness of the insight, not compliance with the specific behavioral ask."

**Data support:**

| Group | n | Churn % |
|---|---|---|
| Ever acted on a summary | 29 | 69.0% |
| Never acted on a summary | 21 | 57.1% |

**Likelihood rank:** 3
**Confidence: 4/10** — the direction here is actually the opposite of the intuitive "acting helps" assumption, which is itself a real, honest, non-cherry-picked finding worth surfacing rather than hiding. But n=29 vs. 21 is small, and this kind of behavioral split is easy to confound (e.g., users under more financial stress might act on nudges more often *and* churn more for unrelated reasons).
**Would confirm it:** A larger sample, or a test that isolates the action step specifically (e.g., randomizing whether the action button is even shown), showing no retention difference between actors and non-actors while the overall treatment-vs-control lift persists.
**Would rule it out:** If a larger sample shows actors retain better once platform and goal-status are statistically controlled for — the small-sample reversal seen here would turn out to be noise or confounding.

### Hypothesis 4 — Users without a savings goal see no benefit because the summary has nothing to reinforce (RANK 4)

**Testable prediction (as originally proposed):** "Users who churned despite opening the summary did not have a savings goal set — without a goal the summary has nothing to make progress on."

**Data support:**

| Group (within week-5 treatment) | n | Churn % |
|---|---|---|
| Set goal in week 1 | 22 | 63.6% |
| No goal in week 1 | 28 | 64.3% |

**Likelihood rank:** 4 (lowest — despite being the most intuitively appealing hypothesis, the data directly contradicts it)
**Confidence: 2/10** — churn rates are essentially identical regardless of goal status within the treatment group (63.6% vs. 64.3%, a rounding error apart) across a reasonably sized split (22 vs. 28 users). If goal absence were a major post-treatment churn driver, we'd expect a real gap here, and there isn't one — even though goal-setting is a strong lever *overall* (Q2: 36.4% vs. 21.8%), it doesn't explain the residual churn specifically among people who already received the treatment.
**Would confirm it:** A larger sample, isolating goal-status specifically among consistent openers (e.g., among users who opened all 4 summaries, do goal-setters churn meaningfully less than non-goal-setters) — if a real gap appears only at higher engagement levels, it would revive this hypothesis in a more specific form.
**Would rule it out:** The data already substantially rules it out at this sample size — this is the one hypothesis where the evidence should actively lower confidence rather than remain a "plausible open question."

---

## 5. Which Hypothesis to Test First

**Test Hypothesis 1 (platform-specific experience gap) first.**

Three reasons, all grounded in what's above, not general instinct:
1. **It has the largest, most specific effect size of any hypothesis here** — a 25-point swing in a real randomized comparison (iOS control vs. iOS treatment) is a much stronger signal than the smaller, more ambiguous splits behind hypotheses 2 and 3.
2. **It's the only hypothesis where the "confirm" step is cheap and immediate** — checking whether Android treatment users show lower open rates or shallower engagement than iOS treatment users is a query against data that's likely already being collected (session screens, summary opens), not a new experiment.
3. **It has an actionable mitigation even before root cause is fully understood** — if Android is where the gap lives, that's independently useful for a scale decision (e.g., prioritize broader iOS rollout now while a design/engineering investigation runs on Android specifically), rather than blocking the whole rollout decision on resolving every hypothesis first.

Hypothesis 4 is worth explicitly *not* prioritizing, even though it's the most "obvious" story — the data already argues against it, and spending next sprint validating a hypothesis the numbers don't support would be a wasted cycle relative to the platform gap, which the data actively points toward.
