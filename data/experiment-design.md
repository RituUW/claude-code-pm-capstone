# Weekly Summary — Full Experiment Design

> **⚠️ Data provenance unconfirmed** — see `data/metric-findings.md`. The pilot numbers referenced below (and the statistical test built on them) inherit this uncertainty.

Purpose: pressure-test the week-5 pilot result (36% vs. 22% day-30 retention, n=50/variant) and design a properly powered follow-up test before recommending scale.

---

## 1. Pilot Significance Check

**What statistical significance means for a go/no-go decision:** it tells you whether an observed difference is large and consistent enough, relative to sample size, that random chance is an unlikely explanation — it does not by itself confirm the effect is real or tell you its true size.

**Test:** two-proportion z-test, 95% confidence.

| | Treatment | Control |
|---|---|---|
| n | 50 | 50 |
| Day-30 retained | 18 | 11 |
| Day-30 rate | 36% | 22% |

- Difference: +14 points
- z = 1.54, two-tailed p ≈ 0.12
- 95% CI on the difference: **≈ −4pp to +32pp**

**Result: Not statistically significant at 95% confidence** (p ≈ 0.12, threshold p < 0.05).

**Plain-English meaning:** The 14-point lift is a real, encouraging observation, but the confidence interval is wide enough to include "control was actually slightly better." At n=50/arm we cannot yet distinguish a genuine effect from a lucky draw. This is not a rejection of the hypothesis — it's a sample-size problem, and it's exactly why a full test is needed before a scale decision.

---

## 2. Minimum Detectable Effect (MDE)

**What it means and why it's set upfront:** MDE is the smallest true effect size the test is designed to reliably catch. It must be chosen before running the test — choosing it after seeing results lets the target effect size be quietly rationalized to match whatever number came out, which defeats the purpose of testing at all.

**MDE for this test: 5 percentage points** — deliberately smaller than the pilot's observed 14pp, since the pilot isn't yet trusted as the true effect size.

---

## 3. Sample Size & Power

**What power means and the risk if it's too low:** power is the probability the test will detect a true effect if one actually exists. Too-low power risks a false negative — running a full test, seeing "no significant difference," and killing a feature that genuinely works.

**Inputs:** baseline = 22%, MDE = 5pp (target treatment rate = 27%), power = 80%, significance = 95% (two-tailed).

**Formula:**
```
n = [(z₀.₀₂₅ + z₀.₂₀)² × (p₁(1−p₁) + p₂(1−p₂))] / (p₁−p₂)²
```

**Result: ≈ 1,160 users per variant (≈ 2,320 total)** — roughly 23x the pilot's size.

---

## 4. Test Duration

**Available:** 85,000 WAU, max 8-week window.

**Constraint analysis:** population size is not the bottleneck (2,320 needed vs. 85,000 available). The bottleneck is time: day-30 retention can't be read until 30 days after the last user enrolls.

**Design:** enroll across **3 weeks** (~774 users/week, split across both arms) rather than a single week, to average out week-to-week noise — a real weakness of the original single-cohort pilot, independent of its small sample.

**Total duration: 3 weeks enrollment + 4.3 weeks (30 days) for the last cohort to mature ≈ 7.3 weeks.**

**Fits within the 8-week constraint: Yes**, with roughly 5 days of buffer for final analysis.

---

## 5. Decision

**Recommendation: run the full test before recommending scale.** The pilot is promising and mechanistically well-supported (session-frequency recovery, per the diagnosis), but fails a 95%-confidence significance check at its current size. A properly powered test is affordable relative to available traffic and fits the 8-week window — there's no forcing function to decide off an underpowered result.

- **Risk of waiting (running the full test):** ~5-7 more weeks of new cohorts experiencing the same baseline retention decline the feature is meant to fix, before a validated, scaled version reaches them.
- **Risk of scaling now (skipping the full test):** if the true effect is smaller than 14pp or null, engineering effort and user-facing rollout go toward a feature that may underdeliver, with a credibility cost if it later needs to be walked back.

---

## 6. Leading Indicators During the Full Test

Monitor weekly, without waiting for the full 7.3-week readout:

1. **Weekly summary open rate, by cohort.** Pilot pattern: 28%→52%→52%→56% across 4 sends. *Get nervous if:* new cohorts don't show the same climb, or it flattens/declines early — undercuts the "engagement builds, doesn't decay" evidence that countered the novelty-decay risk.
2. **Average sessions/user, treatment vs. control, by cohort.** Pilot mechanism: treatment restored sessions/user from ~3.7 to ~5.3. *Get nervous if:* new treatment cohorts don't hold near that range and drift back toward control's ~3.7 — the causal mechanism isn't reproducing.
3. **Day-7 retention, treatment vs. control, by cohort.** Pilot: +30pp (76% vs. 46%), available in 7 days as an early proxy for day-30. *Get nervous if:* the gap on new cohorts shrinks well below that (e.g., under +10-15pp) — an early signal the eventual day-30 effect will undershoot even the 5pp MDE, well before the full test concludes.

---

## Summary Table

| Question | Answer |
|---|---|
| Is the pilot significant at 95%? | No (p ≈ 0.12) |
| MDE | 5 points |
| Required sample size | ≈ 1,160/variant (≈ 2,320 total) |
| Test duration | ≈ 7.3 weeks (fits 8-week max) |
| Recommendation | Run the full test before scaling |
| Leading indicators | Summary open rate, avg sessions/user, day-7 retention (all by cohort, weekly) |
