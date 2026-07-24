# Workspace Audit — Nudge Engage v2

Reviewed: full folder structure, CLAUDE.md, and every file currently in the workspace.

## 1. What's Missing for Next-Session Usefulness

- **CLAUDE.md is frozen at Day 1.** It still describes the "open decision" as unresolved and the direction as "early, being explored." In reality: Marcus approved the direction, a prototype went through 3 iterations and multiple test rounds, a real pilot ran and was analyzed, a PRD was written and pressure-tested, and a quarterly deck is built asking Marcus to fund the next test. A new session reading only CLAUDE.md would badly misjudge where things stand. This is the single biggest gap — fixed in this update.
- **No "current status" snapshot anywhere.** `change_log.md` is a good chronological record, but it's long, and nothing summarizes "here's where we are right now" without reading the whole thing top to bottom. CLAUDE.md is the natural home for this and didn't have it.
- **No file map.** With ~25 files across 5 folders now, there's no index describing what each folder/file is for or which prototype version is canonical. A new session (or a new collaborator) has to open everything to figure out what's current vs. superseded.
- **The source dataset isn't in the workspace.** `data/` holds the *analysis* (`metric-findings.md`, `metric-diagnosis.md`, `experiment-design.md`) but the underlying `Copy of Nudge Dataset.xlsx` lives in `~/Downloads`, outside the project. If anyone needs to re-run or extend the analysis later, the source won't be where the outputs are.
- **No stakeholder index.** Three separate profiles exist (`stakeholders/raj.md`, `lena.md`, `marcus.md`) but nothing points to them from the main context file, and each mixes "confirmed from workspace" facts with "filled from a default profile" assumptions — useful, but only if a reader knows to check which is which.
- **Prototype versioning isn't documented.** `prototype/index.html`, `v2.html`, and `v3.html` all exist with no changelog explaining what changed between them or which is canonical. `README.md` only describes the original v1. `v3` also has 3 known blocking bugs vs. the agreed spec (per `docs/qa-checklist.md`) that haven't been fixed yet — that status isn't visible anywhere at a glance.

## 2. Reorganization That Would Reduce Friction

- **Three overlapping context files have drifted out of sync.** `CLAUDE.md`, `project.md`, and `strategy.md` all cover "what Nudge is / the squad / the hypothesis" with near-duplicate text — and they've already diverged: `project.md` still says "Current Phase: Discovery," while `strategy.md` has all the research findings folded in and is far more current. Recommend: treat `CLAUDE.md` (this update) as the single authoritative, always-current summary going forward. `strategy.md` can stay as a more detailed running log of research findings (it's genuinely useful at that level of detail); `project.md` is now largely redundant with both and safe to either fold into `CLAUDE.md` or stop maintaining separately.
- **`PRD Skeleton.md` at the root is superseded and inconsistently named.** It was the very first draft, before real research existed; `docs/prd.md` is the real, current PRD. Recommend moving the skeleton into `docs/` (or an `archive/` folder) and marking it clearly superseded, rather than leaving it at root level where it reads as equally current.
- **`prototype/README.md` needs a version table.** A one-paragraph addition — which file is canonical (v3), what changed at each version, and the known-bugs status from the QA pass — would save a lot of re-derivation next session.
- **Consider a lightweight `stakeholders/README.md`** (or a short section in CLAUDE.md, which this update includes) pointing to the three profiles and flagging that each mixes confirmed and assumed information.

## 3. CLAUDE.md Update

Rewritten and saved directly to `CLAUDE.md` — now reflects current status (pilot run and analyzed, PRD written and pressure-tested, quarterly deck built, resourcing decision pending with Marcus), adds a "Where Things Live" file map, and points to the stakeholder profiles and prototype version status instead of leaving them undiscoverable.
