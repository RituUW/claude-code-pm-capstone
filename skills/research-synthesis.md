---
name: research-synthesis
description: Weekly synthesis of new user feedback, support tickets, or NPS comments for Nudge Engage v2, pulled from a designated inbox folder with zero manual input. Use when the user pastes the trigger phrase below or otherwise asks for their weekly research synthesis.
---

# Weekly Research Synthesis — One-Command Workflow

## Trigger Prompt

The user pastes exactly this (or a clear paraphrase of it):

> "Run my weekly research synthesis."

No pasted feedback required — this skill reads whatever's waiting in the inbox folder.

## Steps Claude Runs

1. Look in `research/inbox/` for any new files added since the last synthesis was run (check `research/synthesis/` for the most recent prior output to find the cutoff date). This is where raw new inputs — user feedback exports, support ticket dumps, NPS comment exports — get dropped between runs.
2. **If the inbox is empty or has nothing new:** say so plainly in the output ("No new feedback inputs this week") rather than fabricating findings or re-summarizing old material.
3. If there is new material, extract themes using the same method as `research/nps-analysis.md` and `research/interview-synthesis.md`: identify themes mentioned more than once, rank by frequency, separate praise from complaints, and flag any single-mention issue that's high-severity even if not frequent (e.g., a full opt-out, a churn-triggering complaint).
4. Cross-reference against `strategy.md` and `docs/hypothesis.md` — explicitly note whether each theme **confirms**, **contradicts**, or is **new relative to** what's already documented there. Don't just restate existing findings as if they're novel.
5. Identify the top 1-3 actionable items this week's batch surfaces, the same way `research/nps-analysis.md` did — only include an item if the input actually supports it, not a generic restatement of prior weeks' actions.
6. Move or mark processed inbox files (e.g., note in the output which files were included) so the next run doesn't re-process the same material.

## Output Format

```
# Weekly Research Synthesis — [date]

## Source
[files processed, or "No new inputs this week"]

## Themes (ranked by frequency)
...

## Praise vs. Complaints
...

## Confirms / Contradicts / New vs. Existing Research
...

## Top Actionable Items This Week
...
```

## Where the Output Gets Saved

`research/synthesis/YYYY-MM-DD.md` (using the current date). Create the `research/synthesis/` and `research/inbox/` folders if they don't exist yet — `research/inbox/` should be pointed out to the user as the drop location for next week's raw material.
