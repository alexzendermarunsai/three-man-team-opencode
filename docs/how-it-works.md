# How Three Man Team Works

## The Sprint

Every unit of work follows the same path:

**Think → Plan → Build → Review → Deploy → Log**

This is not a suggestion. It is the workflow. Skipping steps is how bugs ship.

## The Roles

**Architect** thinks before anyone builds. They understand the whole system, translate
the Project Owner's intent into a build brief, and own the deploy gate. They are the
bridge between what the Project Owner wants and what Builder produces.

**Builder** builds exactly what the brief says. No more. No less. They show their plan
before building anything non-trivial. They document every change. They stop when the
review request is written.

**Reviewer** reviews what Builder wrote against three things: the brief, correctness,
and standards. They do not pass work that is not right. They do not soften findings.
They escalate product decisions to Architect — never guess.

## How Agents Work Together

Three Man Team uses opencode's agent system with config deep-merge:
- **Architect** is a primary agent — the one you interact with directly in your opencode session.
- **Builder** and **Reviewer** are subagents — Architect delegates to them using opencode's Task tool.
- You run one opencode session. Architect is your main agent. When work is ready to build, Architect spins up Builder. When Builder signals done, Architect spins up Reviewer. All three roles happen inside your single session.

### Config Deep-Merge

opencode deep-merges global config (`~/.config/opencode/`) with project config:
- **Global install**: Agent definitions and skills live at `~/.config/opencode/agents/` and `~/.config/opencode/skills/` — available in every project automatically.
- **Per-project install**: Same files but local to the project's `.opencode/` directory.
- **Project config** (`opencode.json` in project root) sets `default_agent` and project-specific `instructions` — opencode merges these over the global config.
- **Project agents** (`.opencode/agents/*.md`) override same-name global agents — customize personas per-project without touching the global definitions.

## The Files

The team communicates through files, not conversation. This is intentional.

Each role starts a session with a clean context window, reading only the files relevant
to their specific job. Builder never loads the full project spec. Reviewer never loads
the database schema. This is how token discipline becomes structural rather than behavioral.

## The Deploy Gate

Nothing goes to production without Architect's explicit review and the Project Owner's
awareness. The Project Owner knows what is going live. The deploy is documented in
BUILD-LOG. This is accountability, not ceremony.