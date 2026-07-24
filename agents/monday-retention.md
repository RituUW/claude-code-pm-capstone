# Monday Retention Digest — Agent Spec

An agent that runs every Monday morning before standup: checks Nudge 30-day retention against last week's baseline, compares session counts and push/nudge open rates, and posts a plain-English digest to Slack.

**Status:** spec + manually-verified script. Not yet wired to a live scheduler or Slack — see "Path to Production" at the bottom.

---

## 1. The Agent Script

```python
#!/usr/bin/env python3
"""
Monday Retention Digest — Nudge Engage v2

Compares this week's key health metrics against last week's baseline and
produces a plain-English Slack digest.

In production this would query Nudge's data warehouse for a rolling
30-day window ("as of today" vs "as of 7 days ago") and run on a Monday
7am cron/n8n schedule, posting via a Slack webhook. This version reads
the same local dataset used in prior analysis (see data/metric-findings.md)
and compares the two most recent fully-observed weekly cohorts as a
stand-in for "this week vs last week."
"""

import sqlite3
import os

XLSX_PATH = os.path.expanduser("~/Downloads/Copy of Nudge Dataset.xlsx")
DB_PATH = "/tmp/monday_digest.db"

# Stand-in for "current week" vs "last week" until this runs against a
# live rolling window. Update these two lines each week in production,
# or replace with a date-based query once wired to the real warehouse.
CURRENT_WEEK = "4.0"
BASELINE_WEEK = "3.0"


def build_db():
    import openpyxl
    wb = openpyxl.load_workbook(XLSX_PATH, read_only=True, data_only=True)
    con = sqlite3.connect(DB_PATH)
    cur = con.cursor()
    sheet_to_table = {
        "Users": "nudge_users",
        "Sessions": "nudge_sessions",
        "Retention": "nudge_retention",
        "Nudges": "nudge_nudges",
        "Weekly_Summary_Sends": "nudge_weekly_summary_sends",
    }
    for sheet, table in sheet_to_table.items():
        ws = wb[sheet]
        rows = list(ws.iter_rows(values_only=True))
        header, data = rows[0], rows[1:]
        cur.execute(f"DROP TABLE IF EXISTS {table}")
        cols = ", ".join(f'"{c}" TEXT' for c in header)
        cur.execute(f"CREATE TABLE {table} ({cols})")
        placeholders = ", ".join(["?"] * len(header))
        str_data = [[str(v) if v is not None else "" for v in row] for row in data]
        cur.executemany(f"INSERT INTO {table} VALUES ({placeholders})", str_data)
    con.commit()
    return con


def get_retention(cur, week):
    cur.execute(
        "SELECT COUNT(*), SUM(CAST(day_30 AS REAL)) FROM nudge_retention WHERE cohort_week = ?",
        (week,),
    )
    total, retained = cur.fetchone()
    return round(100.0 * retained / total, 1)


def get_avg_sessions(cur, week):
    cur.execute(
        """SELECT COUNT(*), COUNT(DISTINCT s.user_id)
           FROM nudge_sessions s JOIN nudge_users u ON s.user_id = u.user_id
           WHERE u.cohort_week = ?""",
        (week,),
    )
    total_sessions, n_users = cur.fetchone()
    return round(total_sessions / n_users, 2)


def get_nudge_open_rate(cur, week):
    cur.execute(
        """SELECT COUNT(*), SUM(CAST(n.opened AS REAL))
           FROM nudge_nudges n JOIN nudge_users u ON n.user_id = u.user_id
           WHERE u.cohort_week = ?""",
        (week,),
    )
    sent, opened = cur.fetchone()
    return round(100.0 * opened / sent, 1)


def pct_relative_change(current, baseline):
    if baseline == 0:
        return 0.0
    return round(100.0 * (current - baseline) / baseline, 1)


def biggest_mover(metrics):
    # metrics: dict of name -> (current, baseline, relative_change_pct)
    return max(metrics.items(), key=lambda kv: abs(kv[1][2]))


def suggest_action(name, rel_change):
    direction = "dropped" if rel_change < 0 else "rose"
    if name == "push/nudge open rate" and rel_change < -15:
        return (f"Push/nudge open rate {direction} sharply ({rel_change}% relative). "
                f"Review nudge content, timing, and frequency this week — a sharp open-rate "
                f"drop often precedes a broader engagement decline.")
    if name == "avg sessions/user" and rel_change < -10:
        return (f"Sessions per user {direction} {rel_change}% relative to last week. "
                f"Check whether this tracks the retention number or is moving independently — "
                f"if both are falling together, it's the same erosion pattern flagged in "
                f"data/metric-diagnosis.md.")
    if name == "30-day retention" and rel_change < -5:
        return (f"Retention itself is the biggest mover, down {rel_change}% relative. "
                f"Pull this week's cohort composition (acquisition channel, platform) to check "
                f"for a mix shift before assuming a product cause.")
    if rel_change > 5:
        return f"{name} {direction} {rel_change}% relative — worth a quick look, but not urgent."
    return "No metric moved meaningfully this week. Keep the normal weekly cadence."


def main():
    con = build_db()
    cur = con.cursor()

    retention_cur = get_retention(cur, CURRENT_WEEK)
    retention_base = get_retention(cur, BASELINE_WEEK)
    sessions_cur = get_avg_sessions(cur, CURRENT_WEEK)
    sessions_base = get_avg_sessions(cur, BASELINE_WEEK)
    open_cur = get_nudge_open_rate(cur, CURRENT_WEEK)
    open_base = get_nudge_open_rate(cur, BASELINE_WEEK)

    metrics = {
        "30-day retention": (retention_cur, retention_base, pct_relative_change(retention_cur, retention_base)),
        "avg sessions/user": (sessions_cur, sessions_base, pct_relative_change(sessions_cur, sessions_base)),
        "push/nudge open rate": (open_cur, open_base, pct_relative_change(open_cur, open_base)),
    }

    mover_name, (mover_cur, mover_base, mover_rel) = biggest_mover(metrics)
    action = suggest_action(mover_name, mover_rel)

    # In production: format via the Slack template below and POST to a
    # Slack incoming webhook URL instead of printing.
    print("=" * 60)
    print("MONDAY RETENTION DIGEST")
    print("=" * 60)
    print(f"Comparing cohort week {CURRENT_WEEK} vs cohort week {BASELINE_WEEK}\n")
    for name, (cur_v, base_v, rel) in metrics.items():
        print(f"  {name:22s}: {cur_v} (was {base_v}, {rel:+.1f}% relative)")
    print(f"\nBiggest mover: {mover_name} ({mover_rel:+.1f}% relative)")
    print(f"Suggested action: {action}")
    print("=" * 60)

    con.close()


if __name__ == "__main__":
    main()
```

**Notes on the metric mapping used here:**
- "Push notification open rate" is mapped to the `nudge_nudges` table (`nudge_type`: goal_reminder, milestone, re_engagement, spending_alert, weekly_tip) — the dataset's closest analog to push notifications, distinct from the weekly-summary-specific email/in-app send log. If Nudge has a dedicated push-notification table in production, swap the query in `get_nudge_open_rate`.
- "This week vs last week" is currently implemented as **cohort week N vs cohort week N-1** (the dataset's native structure), not a true rolling 30-day window. That's a deliberate stand-in — see "Path to Production" below for what changes when this runs for real.

---

## 2. How to Run It Manually to Verify the Output

1. Ensure `openpyxl` is installed: `pip3 install openpyxl`
2. Save the script above as `monday_retention.py`.
3. Confirm `Copy of Nudge Dataset.xlsx` is in `~/Downloads` (or update `XLSX_PATH` at the top of the script to point at wherever the dataset lives).
4. Run it: `python3 monday_retention.py`
5. Read the printed digest and sanity-check it against `data/metric-findings.md` / `data/metric-diagnosis.md` before trusting it on a real cron schedule.

**Verified output from an actual run (comparing cohort week 4 vs week 3):**

```
============================================================
MONDAY RETENTION DIGEST
============================================================
Comparing cohort week 4.0 vs cohort week 3.0

  30-day retention      : 22.0 (was 25.0, -12.0% relative)
  avg sessions/user     : 3.69 (was 4.48, -17.6% relative)
  push/nudge open rate  : 9.4 (was 14.3, -34.3% relative)

Biggest mover: push/nudge open rate (-34.3% relative)
Suggested action: Push/nudge open rate dropped sharply (-34.3% relative). Review nudge content, timing, and frequency this week — a sharp open-rate drop often precedes a broader engagement decline.
============================================================
```

This is a genuinely new finding, not previously called out in `data/metric-diagnosis.md`: **push/nudge open rate is the single biggest relative mover between these two cohorts (-34.3%), larger than either the retention drop (-12.0%) or the sessions-per-user drop (-17.6%).** Worth a follow-up look at nudge content/cadence specifically, separate from the weekly-summary-specific findings already documented.

---

## 3. Slack Message Template

```
:chart_with_downwards_trend: *Monday Retention Digest* — [DATE]

*Headline:* 30-day retention is at *[X]%*, [up/down] from *[Y]%* last week ([+/-Z]pp).

*Signal to watch:* [METRIC NAME] moved the most this week — [current value] vs [baseline value] last week ([+/-N]% relative).

*This week, take a look at:* [ONE-SENTENCE SUGGESTED ACTION]

_Auto-generated — verify against `data/` before treating as final._
```

**Filled in with the verified run above:**

```
:chart_with_downwards_trend: *Monday Retention Digest* — [DATE]

*Headline:* 30-day retention is at *22.0%*, down from *25.0%* last week (-3.0pp).

*Signal to watch:* Push/nudge open rate moved the most this week — 9.4% vs 14.3% last week (-34.3% relative).

*This week, take a look at:* Review nudge content, timing, and frequency — a sharp open-rate drop often precedes a broader engagement decline.

_Auto-generated — verify against `data/` before treating as final._
```

---

## Path to Production

To actually run this every Monday and post to Slack, three things change:

1. **Data source:** replace `build_db()` reading a local `.xlsx` with a query against Nudge's real data warehouse, computing a true rolling 30-day retention window ("as of today" vs "as of 7 days ago") instead of comparing fixed historical cohort weeks.
2. **Scheduler:** wrap the script in a cron job (`0 7 * * 1` for 7am Monday), an n8n workflow node, or a scheduled cloud function — whichever the team already uses for other recurring jobs.
3. **Delivery:** replace the `print()` calls with a POST request to a Slack incoming webhook URL, using the message template above (Slack `mrkdwn` formatting already matches the template's `*bold*` and `_italic_` syntax).

No other logic changes — the metric queries, biggest-mover detection, and action-suggestion rules stay the same.
