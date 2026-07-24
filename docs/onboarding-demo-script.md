# Claude Code Live Demo Script — 15 Minutes

Run this in the Nudge workspace with your teammate watching, then driving. Have `docs/onboarding-guide.md` open for them to skim beforehand if there's a spare minute — otherwise just start.

---

## Minutes 0-2 — Open the Folder, Say Hello

1. Open a terminal, `cd` into the Nudge project folder.
2. Run `claude` to start a session.
3. **Point out:** Claude's first response (or a quick "what do you know about this project?" prompt) will reflect `CLAUDE.md` — it's loaded automatically, every session, without you asking.
4. **Say out loud:** "This file is the project's memory. Everything Claude says next comes from what's actually in here — not a guess." Scroll to the "Current Status" section in `CLAUDE.md` and read 2-3 lines aloud (retention numbers, where the project actually stands).
5. **Talking point:** "Notice it knows the retention dropped from 44% to 37%, knows the team structure, knows we're waiting on a resourcing decision — none of that was in my prompt just now. It's already there because someone kept this file current."

**Transition line:** "Now let's see what it looks like to actually build something new from scratch, the right way — starting with an interview, not a demand."

---

## Minutes 2-7 — The AI Interview + Plan Mode

1. Open `research/nudge-slack-thread.md` — the `#product-engage` thread where Marcus, Raj, and Lena discuss the retention drop. Paste its contents into Claude (or have your teammate open the file themselves and copy it in).
2. Paste it into Claude with a prompt like: *"Before you create any files, interview me. Ask me one question at a time until you're confident you understand what I need. Then show me the plan and wait for my approval."*
3. **Let Claude ask its first question.** Answer it live, out loud, narrating your reasoning.
4. **Point out:** Claude is not writing anything yet — it's building understanding first. This is the habit from the onboarding guide: interview before building.
5. Once Claude proposes a plan (or enters plan mode, if it does), **stop and read the plan aloud** before approving it.
6. **Talking point:** "This is the moment most people skip — they'd just say 'yes go' the first time. Take 10 seconds to actually check the plan matches what you meant. It's much cheaper to correct now than after 5 files get written."
7. Approve the plan, let it save the output.

**Transition line:** "That's a one-off interview. Now let's look at something you'll use every single week without re-explaining anything."

---

## Minutes 7-12 — Weekly Status Skill (Watch, Then Drive)

1. Open `skills/friday-status.md` briefly — **point out:** this is a saved, reusable instruction set. It's not a one-time chat, it's a skill Claude can run the same way every time.
2. Trigger it yourself: paste *"Run my Friday status update."*
3. **Narrate what's happening:** Claude is reading `change_log.md` itself — you didn't have to type out what shipped this week, it went and found it.
4. Show the two outputs it produces (team update + leadership update) and point out the different tone/length for each audience.
5. **Now hand over the keyboard.**
6. Have your teammate jot 3-4 real bullet points about their own work this week (shipped / in progress / blocked) — doesn't need to be Nudge-related.
7. Have them paste those notes with: *"Turn this into a team update and a leadership update."* (Using `skills/weekly-status.md`, the version that takes raw pasted notes, since they won't have a `change_log.md` of their own yet.)
8. **Let them read their own output.** This is their first "I made Claude do something useful" moment — let it land before moving on.

**Transition line:** "Everything you just watched — the interview, the plan, the skill — came from one prompt we wrote once and reused. Let's give you that same starting point for your own product."

---

## Minutes 12-15 — Their Own Workspace, Their Own First Interview

1. Have them create a new empty folder for their own product idea (real or practice).
2. Open `docs/capstone-session.md` in the Nudge workspace, scroll to the **"Portable Prompt"** section at the bottom.
3. Have them copy the whole prompt block.
4. They `cd` into their new folder, run `claude`, and paste the prompt — filling in their own `[PRODUCT NAME]`, `[CORE METRIC]`, etc. as Claude asks or as the prompt requires.
5. **You stay hands-off from here.** Let Claude start asking them its first real question about their product.
6. **Close with:** "That's it — you now have a project that remembers itself, a habit of interviewing before building, and a folder that'll make sense to you three weeks from now. From here it's just: keep talking to it like you did today."

**Session ends here — with them mid-interview in their own workspace, not watching you drive.**

---

## Presenter Notes

- If Claude's plan-mode behavior differs slightly from what's described (exact UI varies by version), just narrate whatever confirmation step appears — the point is showing "review before build," not a specific mechanism.
- If the teammate gets a boring/simple first Claude answer in minutes 0-2, prompt with something like "what's blocking us right now?" to pull a more concrete, obviously-file-sourced answer from `CLAUDE.md`.
- Keep minutes 7-12 moving — the goal is *they* type something and see real output before the 15 minutes are up, not a perfect demo.
