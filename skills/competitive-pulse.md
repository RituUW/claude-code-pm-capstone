---
name: competitive-pulse
description: Weekly check for new moves from Nudge's tracked competitors (YNAB, Monarch Money, Rocket Money, Copilot Money, Cleo), with zero manual input. Use when the user pastes the trigger phrase below or otherwise asks for a competitive pulse check.
---

# Competitive Pulse Check — One-Command Workflow

## Trigger Prompt

The user pastes exactly this (or a clear paraphrase of it):

> "Run my competitive pulse check."

No competitor list or search terms required — this skill already knows who to track.

## Steps Claude Runs

1. Read `research/competitive-matrix.md` for the tracked competitor list (YNAB, Monarch Money, Rocket Money, Copilot Money, Cleo) and each one's last-known "Notable recent changes" and pricing, so this run has a baseline to compare against.
2. For each competitor, run a web search for recent news, pricing changes, feature launches, app store changelog updates, or notable reviews from roughly the last 7-14 days (e.g., `"<competitor> pricing update"`, `"<competitor> new feature"`, `"<competitor> review <current month/year>"`).
3. Compare findings against the baseline from step 1 — only report something as "new" if it's genuinely not already captured in `competitive-matrix.md` or `competitive-reddit.md`.
4. **If a competitor has no notable new activity found:** say so plainly ("No notable changes found this week") rather than padding the entry with old information restated as current.
5. Flag anything that specifically touches the two white-space gaps already identified in `research/competitive-matrix.md` (passive sustained-novelty personalization; proactive goal follow-through) — a competitor moving into either space is higher-priority than an unrelated update (e.g., a pricing tweak).
6. Do not treat search-tool gaps as "no activity" — if a competitor's coverage was thin (as Copilot Money's was in the original competitive-reddit.md pass), note that the search was inconclusive rather than reporting a false "no change."

## Output Format

```
# Competitive Pulse Check — [date]

## Summary
[1-2 sentences: anything urgent, or "no major movement this week"]

## Per Competitor
- **YNAB:** [new activity, or "No notable changes found"]
- **Monarch Money:** [...]
- **Rocket Money:** [...]
- **Copilot Money:** [...]
- **Cleo:** [...]

## Worth a Deeper Look
[Anything touching Nudge's identified white-space gaps, or explicit "nothing this week"]
```

## Where the Output Gets Saved

`research/competitive-pulse/YYYY-MM-DD.md` (using the current date). Create the `research/competitive-pulse/` folder if it doesn't exist yet.
