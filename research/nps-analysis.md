# NPS Feedback Analysis — Findings Report

Prepared for: Marcus
Source: 10 raw NPS verbatims, Nudge users
Context: Engage v2 discovery — validating the retention hypothesis (30-day retention: 44% → 37%)

## Summary

This feedback strongly corroborates the Engage v2 working hypothesis: the app is compelling on day one but gives users no reason to return afterward. Every multi-mention theme points to the same root cause — a static, generic experience after the initial spending breakdown — and directly supports the personalized weekly summary direction already in discovery.

## Themes Mentioned More Than Once (Ranked by Frequency)

### 1. No reason to return after the first week — 5 mentions
The app doesn't change, so users forget it exists or stop bothering to open it.

- "Nothing made me want to come back." (spending review, then disengagement)
- "The first week was genuinely eye-opening. After that it just felt repetitive."
- "Love the concept. I just forget it exists. If I got one really useful insight a week I would open it every week."
- "The home feed shows the same things every time I open it. There is nothing new to discover."
- "The weekly email is the only thing keeping me engaged. The app itself has not given me a reason to open it."

### 2. Feels generic, not personalized — 2 mentions
Users don't feel Nudge "knows" them; the content reads as interchangeable with any other finance app.

- "I wish it would surface things I do not already know about my spending instead of just confirming what I already suspect."
- "Deleted after 3 weeks. The app did not feel like it knew me at all. Just generic money tips I could find anywhere."

### 3. Wants proactive guidance, not passive tracking — 2 mentions
Users want Nudge to act on their behalf — tell them what to do, follow up on commitments — rather than just report data.

- "I want Nudge to feel like a financial coach, not a spending tracker. Right now it just shows me what happened. I need it to tell me what to do."
- "I set a savings goal but Nudge has never once mentioned it since. It is like it forgot."

### Notable single-mention issue (flagging, not ranked — only 1 mention but high severity)
- "The nudges feel random. I got a notification that I spent $12 on coffee and I just turned off notifications entirely." — One data point, but it's a full opt-out, and it directly touches the engagement-volume-vs-notification-fatigue tension already in our strategy doc. Worth monitoring as we increase nudge frequency.

## Praise vs. Complaints

**Praise:**
- The initial spending breakdown/first week is genuinely compelling ("genuinely eye-opening").
- Users like the core concept of the product ("Love the concept").
- The weekly email is seen as valuable and effective at driving engagement.

**Complaints:**
- No reason to return after week 1 (static, repetitive, forgettable) — 5 mentions
- Feels generic / not personalized — 2 mentions
- Wants proactive coaching, not passive tracking — 2 mentions
- Notifications feel random and irrelevant — 1 mention, led to full opt-out

## Top 3 Actionable Issues

1. **Build the personalized weekly in-app summary.** This is the most frequently cited gap (5 mentions) and users are explicitly asking for exactly this: "one really useful insight a week." It also extends the one channel users say is already working (the weekly email) into the app itself — directly addressing the disconnect Lena flagged earlier.
2. **Prioritize novel insights over confirmatory ones in the ranking logic.** Users want to be told something they don't already know, not have their existing suspicions restated. This should be a design constraint on the ranking logic Raj scoped, not just a data question.
3. **Close the loop on savings goals.** Users who set a goal report Nudge never follows up on it. Goal progress is already part of the proposed weekly summary — this is a low-lift, high-signal fix since the goal data already exists, and it reinforces the finding that setting a goal correlates with lower churn.

## Connection to Existing Hypothesis

This feedback set independently reproduces the core Engage v2 hypothesis and the interview synthesis findings: the app stops feeling relevant after the first week, and the fix needs to give users something to *act on*, not just something to *see*. It also adds two new, more specific inputs for the ranking logic: prioritize novelty over confirmation, and always include goal-progress follow-up when a goal exists.
