# Claude Code Onboarding Guide — 5-Minute Read

## What Claude Code Is

Claude Code is an AI assistant that works directly inside a folder of files on your computer — it can read what's there, write new files, run searches, and remember context across an entire project instead of starting fresh every time you ask it something. Think of it less like a chatbot you copy-paste into, and more like a collaborator sitting in your project folder who actually knows what's in it.

## What You'll Build in Your First Session

You'll use a single "reusable prompt" (in `docs/capstone-session.md`, bottom section) to run a full PM workflow on your own product, start to finish: it sets up a living context file, synthesizes research, runs competitive analysis, drafts a decision brief, builds an interactive prototype, pressure-tests it with simulated stakeholder reviews, analyzes real data if you have it, writes and stress-tests a PRD, and produces leadership-ready communication — all saved as real files you can open and edit like any document. You paste one prompt, and Claude works through the phases with you, asking before moving to the next one.

## The 3 Habits That Matter Most

1. **Update `CLAUDE.md` every session.** This is the one file Claude reads automatically every time you open the project — it's your project's memory. If it goes stale (like ours did after Day 1), Claude starts every session with an outdated picture, even if 50 other files are current. Ask Claude to update it whenever something meaningfully changes.
2. **Use the AI interview before building anything.** Don't hand Claude a vague goal and let it guess. Ask it to interview you first — one question at a time — until it's genuinely confident it understands what you need, and have it show you the plan before it starts. This is the single biggest lever for getting something useful on the first try instead of the third.
3. **Save everything, and ask Claude to log it.** Every real output should land in a real file, in a sensible folder, and get a line in a running change log. If it only exists in the chat, it's gone the moment the session ends — a well-organized workspace is what makes next week's session fast instead of starting from zero.

## The Single Most Common Mistake

**Treating Claude Code like a one-shot answer machine instead of a collaborator you steer.** The mistake looks like: pasting a big vague ask, accepting the first thing Claude produces without pushback, and never coming back to update the context file. The fix is simple — ask for the interview first, review what gets built before saying "yes, save that," and treat `CLAUDE.md` as something you maintain, not something you write once and forget. The workspace stays useful in direct proportion to how much you keep steering it.
