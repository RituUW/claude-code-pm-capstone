---
name: friday-status
description: Compile a Friday status update (shipped, in progress, blocked) for Nudge Engage v2 directly from the workspace, with zero manual input. Use when the user pastes the trigger phrase below or otherwise asks to run their Friday/weekly status update.
---

# Friday Status Update — One-Command Workflow

## Trigger Prompt

The user pastes exactly this (or a clear paraphrase of it):

> "Run my Friday status update."

No raw notes, no additional input required — this skill gathers everything itself.

## Steps Claude Runs

1. Read `change_log.md` and identify every entry dated since the last Friday status update was generated (check `docs/` for the most recent prior status file to find the cutoff; if none exists yet, use the last 7 days).
2. Classify each entry into **Shipped** (something completed and saved this week), **In Progress** (explicitly partial, e.g. "70% done," "draft," "first round"), or **Blocked** (anything flagged as waiting on a person, a decision, or an estimate).
3. Cross-check `docs/` for anything created or updated in the same window that the change log entry might under-describe (e.g., a new file appearing without a matching log line) — don't rely on the log alone if a file's timestamp suggests otherwise.
4. Do not invent progress, dates, or owners not present in the workspace. If something is ambiguous (e.g., partially logged), note it as unclear rather than guessing.
5. Generate both outputs described below in a single pass — do not ask which one is wanted.

## Output Format

Two updates, using the same dual-audience structure as `skills/weekly-status.md`:

**1. Team update** (for Raj and Lena) — conversational, addressed to them directly, calls out anything either of them specifically needs to know or act on.

**2. Leadership update** (for Marcus) — exactly:
```
Done: [one or two sentences]
In progress: [one line]
Blocker: [the one blocker that matters most]
Where we stand: [one sentence — status + the one thing that could change the timeline]
```

## Where the Output Gets Saved

`status-updates/YYYY-MM-DD.md` (using the current date), containing both the team update and the leadership update in one file, clearly labeled as two sections. Create the `status-updates/` folder if it doesn't exist yet.
