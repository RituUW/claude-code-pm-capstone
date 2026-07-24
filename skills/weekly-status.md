---
name: weekly-status
description: Turn raw bullet-point notes into audience-specific weekly status updates — a conversational team update and a terse leadership update. Use when the user provides rough notes (shipped/in progress/blockers) and asks for a weekly status update, standup notes, or leadership update.
---

# Weekly Status Update

Convert raw, unstructured bullet-point notes into two different status updates from the same input — one for the working team, one for leadership. Same underlying facts, different altitude and tone.

## Input

Raw bullet-point notes from the user — unstructured, may be in any order, may mix completed work, in-progress work, and blockers together. Typically grouped as Shipped / In Progress / Blockers, but don't assume the input is already sorted correctly — re-sort if something's miscategorized.

## Output 1: Team Update (for peers/collaborators named in the notes)

Conversational, specific, written *to* the people involved by name where relevant.

- Open with a short, direct greeting/framing line — not a header-only doc.
- Cover shipped, in-progress, and blockers in flowing sentences, not rigid bullet sections — this is a message, not a report.
- **Call out anything a specific named person needs to do or knows about** — e.g., a blocker they flagged, a decision waiting on them. Address them directly ("One thing I need from you, X...").
- Include operational context the team needs (e.g., someone being out, a scheduling conflict) even if it wouldn't matter to leadership.
- Plain language, no jargon — but can be warmer/looser than the leadership version.

## Output 2: Leadership Update (for a manager/exec named in the notes)

Terse, scannable, structured exactly as:

```
Done: [everything shipped, combined into one or two sentences]
In progress: [current work, one line]
Blocker: [the one blocker that matters, one line]
Where we stand: [one sentence — overall status + the one thing that could change the timeline]
```

- **Maximum one line per field.** If there are multiple shipped items, combine them into a single sentence — don't list more than what fits on one line.
- If there are multiple blockers, pick the one most likely to affect timeline or need leadership's help — mention others only if truly load-bearing.
- No editorializing beyond what "where we stand" allows — that line is the one place a light forward-looking read is appropriate (e.g., "on track," "at risk," "ahead of plan").
- Skip anything operational/internal (e.g., a teammate's PTO) unless it directly affects a deliverable leadership is tracking.

## Rules (both outputs)

- **Plain, declarative language.** No jargon, no buzzwords, no filler phrases ("leveraging," "circling back," "synergy," etc.).
- **Don't invent information.** Only reorganize and rewrite what's in the raw notes — if a date, owner, or number isn't given, don't make one up (use a placeholder like `[date]` if the format needs one, and flag it as unspecified rather than guessing).
- If a section has no relevant input (e.g., no blockers), say so plainly — `None this week` — don't pad it.
- Produce both outputs every time this skill runs, even if only one was explicitly requested — the point of the skill is the two-audience pairing.
