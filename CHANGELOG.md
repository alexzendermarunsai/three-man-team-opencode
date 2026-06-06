# Changelog

## v2.0.0 — 2026-06-06

- Port: Three Man Team now runs on [opencode](https://opencode.ai) instead of Claude Code
- Config: `CLAUDE.md` removed, replaced by `opencode.json` and `.opencode/` directory structure
- Agents: Architect, Builder, and Reviewer now defined as `.opencode/agent/*.md` files with YAML frontmatter (mode, model, description)
- Architecture: Architect is a `primary` agent, Builder and Reviewer are `subagent` agents
- Delegation: Architect delegates to Builder/Reviewer via opencode's Task tool instead of Claude Code's Agent tool
- Skills: token-optimizer moved to `.opencode/skills/token-optimization/SKILL.md` with proper frontmatter
- Handoff: `SESSION-CHECKPOINT.md` resume prompt updated for opencode's agent switching (Tab key)
- Setup: setup script rewritten to copy `.opencode/` structure and `opencode.json` instead of root-level agent files
- Models: Builder and Reviewer default to `anthropic/claude-sonnet-4-6` (provider-prefix format)
- Deleted: root-level `CLAUDE.md`, root-level `setup`, `.claude/skills/`, `_temp/`, per-template `ARCHITECT.md`, `BUILDER.md`, `REVIEWER.md`
- Docs: README, METHODOLOGY, how-it-works, token-optimization, sprint-walkthrough, session-start, INSTALL all updated for opencode

## v1.2.5 — 2026-06-04

- Fix: RICHARD.md → REVIEWER.md in Richard spinup prompt — Richard was told to read a file that doesn't exist
- Fix: BOB.md → BUILDER.md in root-level template — v1.2.4 fix missed this install path
- Fix: version check upgraded to full procedure — tracks version_notified, fetches release notes, walks migration steps conversationally
- Fix: removed fabricated DeepMind citation from METHODOLOGY.md, README.md, and index.html — replaced with first-principles reasoning
- Improve: ARCHITECT.md Job 1 now documents Diagnose vs Direction modes
- Improve: BUILDER.md REVIEW-REQUEST.md format is now explicit with sub-bullets
- Improve: REVIEWER.md section title simplified
- Improve: CLAUDE.md redundant handoff file list removed
- Docs: star count updated to 813, benchmark placeholder removed from index.html

## v1.2.4 — 2026-05-27

- Fixed: version check tag extraction was silently broken for all v1.2.3 installs ([#8](https://github.com/russelleNVy/three-man-team/issues/8))
- Fixed: typo in Bob spinup prompt (BOB.md → BUILDER.md)

## v1.2.3 — 2026-05-03

- Auto-update check: Arch now checks the GitHub releases API at session start and notifies the Project Owner if a newer version is available
- Added `VERSION` file to `templates/project-folder/` — tracks installed version for comparison

## v1.2.2 — 2026-05-03

- Fix: token-optimization.md now ships with every install — added to `templates/project-folder/.claude/skills/` so the `@` auto-load reference works out of the box
- Fix: CLAUDE.md creation instructions in `new-setup.md` now include `@.claude/skills/token-optimization.md` for both new and existing project context files
- Fix: install `cp` command changed from `*` to `.` in setup script, README, and INSTALL.md — hidden directories (`.claude/`) are now copied correctly
- Docs: added "one session, three roles" callout to README Quick Start and INSTALL.md — clarifies that Bob and Richard are subagents within a single Claude Code session, not separate windows
- Docs: `new-setup.md` now shows the "one session" model explanation to the Project Owner during first-time setup

## v1.2.0 — 2026-04-20

- RTK install block: dropped curl command, now links to github.com/rtk-ai/rtk README (fixes private fork URL)
- Windows support: added Windows section to INSTALL.md — Git Bash/WSL for setup script, manual copy fallback, RTK not supported on Windows
- RTK install note in new-setup.md clarifies macOS/Linux only; Windows users can skip
- handoff/ paths: all agent files now reference handoff/ARCHITECT-BRIEF.md, handoff/REVIEW-REQUEST.md, etc. — prevents files landing in project root
- BUILD-LOG discipline: added Anti-Drift rule requiring BUILD-LOG updates immediately on any decision, not only at deploy
- Model assignment: new-setup.md adds 4th setup question for per-agent model selection; ARCHITECT.md briefing sections now document the `model` parameter with available Claude model IDs

## v1.1.0 — 2026-04-03

- Added `new-setup.md` — guided first-session onboarding handled by Arch: team naming, project context file, RTK install
- Architect now spins up Builder and Reviewer via Agent tool (foreground only) — documented in ARCHITECT.md and INSTALL.md
- Foreground-only requirement documented — background agents stall on Edit approval
- Reviewer protocol upgraded: APPROVED / APPROVED WITH CONDITIONS / REJECTED replaces Must Fix / Should Fix
- Reviewer now runs `git diff` first — diff is primary source of truth, not REVIEW-REQUEST
- Builder self-review and linting gate added before handing off to Reviewer
- Setup script rewritten as guided walkthrough — detects global vs per-project, prints tailored instructions
- `handoff/` folder added to both templates so `cp` command includes it
- `CLAUDE.md` removed from templates — setup script is the only source of truth for that step
- `team.yml.example` removed — renaming handled by Arch during setup
- Stale docs removed: `customizing-your-team.md`, `project-setup.md`
- README Quick Start restructured — two clearly labeled install paths
- Path mismatch in `CLAUDE.md` session router resolved

## v1.0.0 — 2026-03-31

Initial public release.

- Three-agent team: Architect, Builder, Reviewer
- Generic agents in `agents/` with customizable personas and [CUSTOMIZE] placeholders
- Named persona template in `templates/project-folder/` (Arch, Bob, Richard)
- Generic clean-slate template in `templates/generic/`
- Structured handoff files: ARCHITECT-BRIEF, REVIEW-REQUEST, REVIEW-FEEDBACK, BUILD-LOG, SESSION-CHECKPOINT
- Token optimization rules baked into every session router
- RTK integration guidance in docs/token-optimization.md
- Setup script with CLAUDE.md instructions printed on install
- Full documentation suite
