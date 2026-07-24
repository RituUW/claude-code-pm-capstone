# Nudge Weekly Summary — Metric Findings

> **⚠️ Data provenance unconfirmed:** it has not been verified whether `Copy of Nudge Dataset.xlsx` is real production data or an illustrative/exercise dataset. Every number in this document, and everything built on top of it, inherits this uncertainty. Confirm provenance before presenting any of this as a real business result.

Source: `Copy of Nudge Dataset.xlsx` (5 tabs: Users, Sessions, Retention, Nudges, Weekly_Summary_Sends), loaded into SQLite for querying.

Note on access: the originally shared Google Sheet links could not be opened via the connected Drive account (file not found / not shared with the connected account on either link provided). Analysis below uses the local `.xlsx` export instead.

Table naming below follows the task's schema names (`nudge_users`, `nudge_sessions`, `nudge_retention`, `nudge_nudges`, `nudge_weekly_summary_sends`) mapped to the workbook's actual tab names (Users, Sessions, Retention, Nudges, Weekly_Summary_Sends).

---

## 1. 30-Day Retention by Cohort Week

**SQL:**
```sql
SELECT
  cohort_week,
  COUNT(*) AS total_users,
  SUM(CAST(day_30 AS REAL)) AS retained_day_30,
  ROUND(100.0 * SUM(CAST(day_30 AS REAL)) / COUNT(*), 1) AS retention_pct
FROM nudge_retention
GROUP BY cohort_week
ORDER BY cohort_week;
```

**Plain English:** For each weekly signup cohort, count how many users were still active at day 30 versus the total cohort size, and turn that into a percentage.

**Result:**

| Cohort Week | Total Users | Retained (Day 30) | Retention % |
|---|---|---|---|
| 1 | 100 | 32 | 32.0% |
| 2 | 100 | 25 | 25.0% |
| 3 | 100 | 25 | 25.0% |
| 4 | 100 | 22 | 22.0% |
| 5 | 100 | 29 | 29.0% |

**What the decline looks like:** A steady erosion from week 1 (32%) down to week 4 (22%) — a 10-point drop over 3 weeks, consistent with the 44%→37% company-wide decline already documented in `docs/decision-brief.md`. Week 5 breaks the downward trend (29%), but that's expected and shouldn't be read as organic recovery — week 5 is the only cohort with the weekly-summary experiment running, and half of it (the treatment group) is pulling the average up. See Q3 for the split.

**What this means for the scale decision:** The pre-experiment decline (weeks 1-4) is real and worsening, which is exactly the retention emergency the weekly summary was proposed to address — it reinforces urgency, not by itself an argument for or against this specific feature.

---

## 2. Savings Goal in Week 1 vs. Retention

**SQL:**
```sql
SELECT
  CASE
    WHEN u.goal_set_date != '' AND julianday(u.goal_set_date) - julianday(u.signup_date) <= 7
    THEN 'goal_set_week1'
    ELSE 'no_goal_week1'
  END AS goal_group,
  COUNT(*) AS total_users,
  SUM(CAST(r.day_30 AS REAL)) AS retained_day_30,
  ROUND(100.0 * SUM(CAST(r.day_30 AS REAL)) / COUNT(*), 1) AS retention_pct
FROM nudge_users u
JOIN nudge_retention r ON u.user_id = r.user_id
GROUP BY goal_group;
```

**Plain English:** Join users to their retention record. Split users into two groups based on whether their `goal_set_date` is within 7 days of `signup_date` (or blank/later = no goal in week 1). Compare day-30 retention rate between the two groups.

**Result:**

| Group | Total Users | Retained (Day 30) | Retention % |
|---|---|---|---|
| Set goal in week 1 | 165 | 60 | 36.4% |
| No goal in week 1 | 335 | 73 | 21.8% |

**What this means:** Yes — users who set a savings goal in week 1 retain at roughly **1.7x the rate** of those who don't (36.4% vs. 21.8%). This directly confirms the original data pull referenced throughout the project docs (Raj's "~2x churn rate" finding) — the actual multiplier here is closer to 1.7x than a clean 2x, but the direction and magnitude are consistent and strongly significant given the sample sizes (165 vs. 335 users). This is independent of the weekly summary experiment (it holds across all cohorts, not just week 5) and continues to support goal-setting as a high-leverage lever, whether or not the weekly summary ships.

---

## 3. Week 5 Only: Day-7 and Day-30 Retention, Treatment vs. Control

**SQL:**
```sql
SELECT
  u.variant,
  COUNT(*) AS total_users,
  SUM(CAST(r.day_7 AS REAL)) AS retained_day_7,
  ROUND(100.0 * SUM(CAST(r.day_7 AS REAL)) / COUNT(*), 1) AS day7_retention_pct,
  SUM(CAST(r.day_30 AS REAL)) AS retained_day_30,
  ROUND(100.0 * SUM(CAST(r.day_30 AS REAL)) / COUNT(*), 1) AS day30_retention_pct
FROM nudge_users u
JOIN nudge_retention r ON u.user_id = r.user_id
WHERE u.cohort_week = '5.0'
GROUP BY u.variant;
```

**Plain English:** Restrict to week-5 users only (the only cohort with the experiment active), join to retention, and compare day-7 and day-30 retention between the `summary_v1` (treatment) and `control` groups.

**Result:**

| Variant | Total Users | Day-7 Retained | Day-7 % | Day-30 Retained | Day-30 % |
|---|---|---|---|---|---|
| Control | 50 | 23 | 46.0% | 11 | 22.0% |
| Treatment (summary_v1) | 50 | 38 | 76.0% | 18 | 36.0% |

**What this means:** A large, consistent lift in both windows — **+30 percentage points at day 7** (76% vs. 46%) and **+14 percentage points at day 30** (36% vs. 22%). The day-30 treatment rate (36%) is also notably close to the day-30 retention rate for users who set a goal in week 1 (36.4%, from Q2) — suggestive that the weekly summary may be having a similar-magnitude effect to the strongest lever already identified in the research, though this is a single 50-vs-50 cohort and should be treated as an early, promising signal rather than a settled result.

**Caveat to flag honestly:** n=50 per arm is a small sample for a launch decision. The effect size is large enough to be encouraging, but this alone shouldn't be the sole basis for a full rollout decision — worth checking with whoever owns experiment design whether this reaches statistical significance and whether a larger or longer test is warranted before scaling to 100% of new users.

---

## 4. Weekly Summary Open Rate Across the 4 Sends, Treatment vs. Control

**SQL:**
```sql
SELECT
  week_number,
  variant,
  COUNT(*) AS total_sends,
  SUM(CAST(opened AS REAL)) AS total_opened,
  ROUND(100.0 * SUM(CAST(opened AS REAL)) / COUNT(*), 1) AS open_rate_pct
FROM nudge_weekly_summary_sends
GROUP BY week_number, variant
ORDER BY week_number, variant;
```

**Plain English:** For each of the 4 weekly sends, and separately for each variant, count how many sends were opened versus total sends, as a percentage.

**Result:**

| Send # | Control Open % | Treatment (summary_v1) Open % |
|---|---|---|
| 1 | 4.0% | 28.0% |
| 2 | 4.0% | 52.0% |
| 3 | 4.0% | 52.0% |
| 4 | 6.0% | 56.0% |

**What this means:** Yes, clearly. Treatment open rate **climbed across the 4 sends** — 28% → 52% → 52% → 56% — while control stayed essentially flat at 4-6% throughout. Two things stand out: (1) the treatment group's open rate is already 7-14x control's from send 1 onward, meaning the redesigned in-app-continuation experience is dramatically more compelling than whatever control users received; and (2) the treatment open rate *itself* improved after the first send and then held — consistent with the "the app stopped feeling relevant" hypothesis: once users saw the new experience deliver real value once, more of them came back for send 2 onward, and that gain was sustained (not a one-time novelty spike that decayed).

---

## Overall Read for the Scale Decision

All four results point the same direction and reinforce each other:
- The underlying retention decline (Q1) is real and was getting worse before this experiment started.
- Goal-setting (Q2) is confirmed as a strong, independent retention lever — worth continuing to invest in regardless of this feature's outcome.
- The weekly summary experiment (Q3) shows a large lift in both day-7 and day-30 retention for a real, randomized treatment-vs-control split, not just a correlational read.
- Engagement with the weekly summary itself is climbing, not decaying, across its first 4 weeks in market (Q4) — early evidence against the "novelty wears off" risk flagged in the competitive research (Cleo's engagement decay).

**Recommendation grounded in this data:** the results support moving forward with a larger or longer test before a full rollout, not skipping straight to 100% scale — the week-5 cohort is a single 50/50 split, and confirming the effect holds at greater scale and over more weeks is a reasonable, low-cost next gate before committing engineering resources to the full build scoped in `docs/spec-readiness.md`.
