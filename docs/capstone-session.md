# Capstone Review Session — Nudge Engage v2

A full review of everything built across this project, confidence-scored, with fixes proposed and a portable prompt for reuse on a different product.

---

## 1. What's Been Built

**Discovery & Research** (`research/`)
- User interview synthesis (Priya, Tom, Amara) — 5 themes, contradictions/tensions, single most important week-1 insight.
- NPS analysis — 10 verbatims, themes ranked by frequency, top 3 actionable issues.
- Competitive matrix — 5 apps (YNAB, Monarch, Rocket Money, Copilot, Cleo), 2 white-space gaps.
- Competitive Reddit sentiment — validated (and one gap not yet externally validated) against real community sentiment.

**Strategic Framing** (`docs/`, root)
- Decision brief for Marcus — situation, findings, options, recommendation.
- Learning synthesis + formal hypothesis statement (`docs/hypothesis.md`).
- PRD (`docs/prd.md`) — problem, user, goals/non-goals, success metrics, user stories, open questions.
- PRD objection log — 3 simulated reviewers (Raj, Marcus, Tom), highest kill-risk identified.

**Prototype** (`prototype/`)
- v1 → v2 → v3, iterated against real usability sessions (2 live participants) and a synthetic persona-interview round.
- v3 is most complete but has 3 known, unfixed bugs vs. the agreed spec.

**Cross-Functional Alignment** (`docs/`, `stakeholders/`)
- Stakeholder profiles for Raj, Lena, Marcus — confirmed facts vs. assumed defaults, clearly separated.
- Simulated spec review with Raj (`spec-readiness.md`) — resolved data model, ranking-logic definition, fallback tiers, mid-week cut, instrumentation.
- Simulated design review with Lena (`design-review.md`) — identified the happy-path-only risk.
- Triad session prep (agenda, alignment template, Slack invite).
- QA checklist — real code-level trace of v3, found 3 blocking issues.

**Data & Experimentation** (`data/`)
- Pilot metric findings (SQL against the real dataset) — 4 core questions answered.
- Full diagnosis — metric tree, mechanistic explanation of the weeks 1-4 decline, 4 ranked/scored churn hypotheses.
- Experiment design — significance test on the pilot (not significant, p≈0.12), MDE/power/sample-size calculation, duration check, leading indicators.

**Leadership Communication** (`docs/`)
- Recommendation memo for Marcus (situation/evidence/recommendation/ask/risk) + simulated hardest questions.
- 6-slide quarterly deck + full speaker notes.

**Reusable Infrastructure** (`skills/`, `agents/`)
- `weekly-status.md` — dual-audience (team + leadership) status skill.
- `friday-status.md`, `research-synthesis.md`, `competitive-pulse.md` — three one-command, zero-input workflows.
- `agents/monday-retention.md` — a built-and-verified Python script comparing week-over-week metrics, with a Slack template and a documented path to production.

**Workspace Hygiene**
- `workspace-audit.md` — full audit of gaps and reorganization needs.
- `CLAUDE.md` — rewritten to reflect current status with a file map (was previously frozen at Day 1).

---

## 2. Confidence Level per Artifact

| Artifact | Confidence | What would get it to 95% |
|---|---|---|
| Interview synthesis, NPS analysis | **High** | These are direct syntheses of given source material with no external dependency — already close to ceiling. |
| Competitive matrix | **Medium-High** | Built from live web search; pricing/feature claims are only as current as the search results — a scheduled re-check (the `competitive-pulse` skill) would keep this from going stale. |
| Competitive Reddit sentiment | **Medium** | Explicitly built from secondary aggregator sources, not raw Reddit threads (`site:` search wasn't available), and Copilot Money's coverage was thin by the doc's own admission. A direct Reddit API pass would close this gap. |
| Decision brief, hypothesis doc | **Medium-High** | Inherits the ceiling of the research above; internally consistent and well-cited. |
| Prototype v1 | **Medium** | Tested with 2 real usability participants — real signal, but n=2 is small. A larger usability round would raise this. |
| Prototype v2/v3 | **Low-Medium** | v2 partially tested; v3's changes came from a *synthetic* AI-roleplayed persona session, not real users — explicitly flagged as unvalidated in `docs/iteration-log.md`. Needs a real usability round before trusting it. |
| Spec readiness (Raj roleplay), design review (Lena roleplay) | **Medium** | Useful for surfacing the *right questions*, but it's a simulation, not the real Raj/Lena. Needs an actual review with the real people before being treated as resolved. |
| QA checklist | **High** (on the bugs found) | I traced the actual code line-by-line for this one — the 3 bugs found are real and verifiable by re-opening `v3.html`. Confidence is high on *what was found*; lower on completeness (a solo PM pass isn't a substitute for full engineering QA). |
| Pilot data analysis (`metric-findings.md`, `metric-diagnosis.md`) | **Medium** | The SQL and stats are correct and verifiable — but **I don't have independent confirmation that `Copy of Nudge Dataset.xlsx` is real production data rather than a constructed exercise dataset.** This is the single biggest open question affecting confidence across everything downstream of it. |
| Experiment design | **High** (on the math) | Standard, correctly-applied statistical formulas — verifiable by anyone who checks the arithmetic. Depends on the same data-provenance question above for real-world applicability. |
| Recommendation memo, PRD, objection log, presentation | **Medium** | Well-constructed syntheses, but their confidence ceiling is capped by the artifacts they're built on (especially the data-provenance question). |
| Stakeholder profiles | **Medium** | Each file already self-flags confirmed-vs-assumed content — appropriately honest, but the "assumed" half is still a guess until the real people confirm or correct it. |
| Skills (`weekly-status`, `friday-status`, `research-synthesis`, `competitive-pulse`) | **Low-Medium** | `weekly-status` has been run once for real in this session; the other two have never been run. Confidence rises with each real use. |
| `agents/monday-retention.md` | **Medium-High** | The script was actually built and run against real data, not just described — but it explicitly stands in cohort-week comparison for a true rolling-window query, and isn't wired to a live scheduler or Slack yet. |
| `CLAUDE.md`, `workspace-audit.md` | **High** | Self-referential meta-documents; confidence is tied to the thoroughness of the audit itself, which cross-checked every file in the workspace directly. |

---

## 3. What's Worth Fixing Before Handing This to a Real Collaborator

**Fix 1 — The 3 known bugs in `prototype/v3.html`.**
These directly contradict decisions already agreed with "Raj" in `spec-readiness.md`: the Savings Goals card still shows two goals when v1 scope was locked to one; the nudge copy still promises mid-week alerting that was explicitly cut to backlog; the transaction drill-down's total is ambiguous in a way that undermines the exact trust feature it exists to build. Handing this to a real engineer or designer as-is would visibly contradict the very docs sitting next to it in the same folder.

**Fix 2 — Root-level file staleness and redundancy.**
`project.md` still says "Current Phase: Discovery" even though the project has moved well past that; `PRD Skeleton.md` sits at root looking equally current as `docs/prd.md`, which actually supersedes it. A real collaborator opening the repo cold could easily read the wrong file first and draw a stale conclusion.

**Fix 3 — No data-provenance disclaimer on the pilot/experiment work.**
Every number in `data/metric-findings.md`, `metric-diagnosis.md`, `experiment-design.md`, and everything built on top of them (`recommendation-memo.md`, the quarterly deck) rests on `Copy of Nudge Dataset.xlsx`. Nothing in the workspace states whether this is real production data or an illustrative/exercise dataset. Handing a confident, statistically-rigorous recommendation to a real collaborator without that caveat is a real credibility risk if it turns out to be sample data.

*(See below for proposed fixes and a request for approval on each.)*

---

## 4. Portable Prompt — Recreate This Workspace for a Different Product

```
I'm a PM working on [PRODUCT NAME], a [one-sentence product description].
We're seeing [CORE METRIC] decline from [BASELINE] to [CURRENT VALUE] over
[TIME PERIOD], and I want to run a full discovery-to-decision workflow on
this problem, the same way a strong PM would.

Work with me through these phases, in order, saving each output to files
as we go so the workspace stays navigable for future sessions:

1. Set up a living CLAUDE.md with product context, team/stakeholders, the
   core problem, and a "current status" section you'll keep updated as we go.
2. Synthesize qualitative research I'll give you (interviews, NPS/support
   feedback) into theme-ranked, quote-cited findings docs — no invented
   opinions, only what's actually in the source material.
3. Run competitive research — a structured matrix of relevant competitors
   plus real community sentiment (not just marketing pages) — and identify
   white-space gaps we could own.
4. Write a decision brief synthesizing all research into a recommended
   direction, with an honest "why now."
5. Build an interactive prototype (plain HTML/CSS/JS, no build tools) for
   the proposed direction, and iterate it through real and/or simulated
   usability testing rounds — always tell me honestly which is which.
6. Pressure-test the spec and design with simulated stakeholder reviews
   (build stakeholder profiles first, separating confirmed facts from
   assumed defaults), and with a PM-level QA pass that actually traces
   the prototype's code, not just eyeballs it.
7. If I give you real usage data, analyze it rigorously — run actual
   statistical tests (significance, power, sample size), not just
   descriptive stats, and design a properly-powered follow-up test if
   the initial data is promising but underpowered.
8. Write a PRD, then pressure-test it with simulated skeptical reviewers
   from at least three angles: engineering feasibility, business/strategic
   fit, and an actual end user — tell me which objection is most likely
   to kill the initiative if I don't address it first.
9. Build leadership communication artifacts (a memo, a slide narrative
   with full speaker notes) that lead with the recommendation, are honest
   about statistical limitations, and don't oversell.
10. Build reusable skills/agents for my recurring PM workflows (status
    updates, research synthesis, competitive monitoring) — designed to
    run from a single prompt with no follow-up input required, and
    actually test/run each one before calling it done, not just describe it.
11. Periodically audit the whole workspace: what's missing, what's stale,
    what should be reorganized — and keep CLAUDE.md in sync with reality.

Throughout: cite sources for every claim, separate what's confirmed from
what's assumed, flag your own confidence level honestly, and never present
a small or synthetic sample as more conclusive than it is. When you finish
each phase, ask whether to proceed to the next one rather than doing
everything at once.
```

---

## Fixes Proposed — Awaiting Approval

See the chat response for each fix with an explicit yes/no ask before implementing.
