<p align="center">
  <img src="assets/banner.png" alt="Three Man Team" width="100%">
</p>

<p align="center">
  <a href="https://russellenvy.github.io/three-man-team/">russellenvy.github.io/three-man-team</a>
</p>

<p align="center">
  By <a href="https://russellenvy.com">RUSSΞLL AARØN</a>
</p>

---

## What's New — v2.0.0

- **Ported to opencode.** Three Man Team now runs on [opencode](https://opencode.ai) — the open source AI coding agent. No longer requires Claude Code.
- **Two-template architecture** — `templates/global/` (agents + skills + config for `~/.config/opencode/`) and `templates/project/` (handoff state + project config). opencode deep-merges them automatically.
- Agent definitions moved to `.opencode/agents/*.md` / `~/.config/opencode/agents/*.md` with YAML frontmatter (mode, model, description)
- Config replaced: `CLAUDE.md` removed → `opencode.json` + `.opencode/` directory structure
- Architect is a `primary` agent, Builder and Reviewer are `subagent` agents
- Delegation uses opencode's Task tool instead of Claude Code's Agent tool
- Token-optimizer skill moved to `.opencode/skills/token-optimization/SKILL.md`

See [all releases →](https://github.com/alexzendermarunsai/three-man-team-opencode/releases)

---

## The Problem With AI Coding Tools

AI coding tools are powerful but undisciplined. They read entire codebases when they
need one function. They add features nobody asked for. They drift mid-task. They burn
tokens on every session doing work that didn't need to happen.

The solution isn't a better prompt. It's a process.

Three Man Team gives you three agents with distinct jobs, clear handoffs, and rules that prevent the most expensive failure modes. The Architect plans and deploys. The Builder builds exactly what the brief says. The Reviewer doesn't pass work that isn't right.

---

## Why Three Agents

Three is not arbitrary. Solo agents drift — there's no one to catch a wrong turn. Large
teams generate coordination overhead that eats the productivity gain. Three is the minimum
for meaningful review and the maximum before the team starts managing itself instead of
the work.

The roles map to how real software ships:
- Someone who understands the whole system and owns the deploy
- Someone who builds fast and clean
- Someone who catches what the builder missed

---

## Quick Start

**How the team runs:** Three Man Team uses one opencode session. Architect is your main agent. When work is ready to build, Architect delegates to Builder as a subagent via opencode's Task tool. When Builder is done, Architect delegates to Reviewer the same way. You don't open three windows — everything runs inside your single session.

Clone the repo anywhere, then run setup:

```bash
git clone https://github.com/alexzendermarunsai/three-man-team-opencode.git
cd three-man-team-opencode
```

---

### Global install (recommended for multi-project use)

Install once — agents and skill go into `~/.config/opencode/` and are available in every opencode project via deep-merge.

```bash
./setup global
```

Then for each project, setup auto-detects your global install and only copies project state files + merges `opencode.json` (never overwrites existing project config):

```bash
./setup project /path/to/your/project
```

---

### Per-project install (single project only)

If you haven't done a global install, the same command copies everything including agents into the project's `.opencode/`:

```bash
./setup project /path/to/your/project
```

Setup auto-detects whether a global install exists and adjusts what it copies. No need to think about which mode you're in — one command does the right thing.

---

### Starting a session

After setup, run `opencode` in your project, switch to the architect agent (press Tab), and say:

```
Read new-setup.md.
```

---

### Updates

```bash
git pull
./setup global        # re-deploy global agents + skill
./setup project /path/to/project  # re-deploy project files + merge opencode.json
```

## The Workflow

<p align="center">
  <img src="assets/workflow.png" alt="Three Man Team Workflow" width="100%">
</p>

Every unit of work follows the same path. Architect plans and writes the brief. Builder reads it, shows a plan, builds, and hands off to Reviewer. Reviewer clears it or sends it back. Architect deploys with the Project Owner's go-ahead. Nothing skips a step.

See a complete example from problem to deploy → [`examples/sprint-walkthrough.md`](examples/sprint-walkthrough.md)

---

## The Team

<p align="center">
  <img src="assets/role-cards-cropped.png" alt="Arch, Bob and Richard" width="100%">
</p>

Three agents. Three distinct jobs. Built to work together.

Architect, Builder, Reviewer are the defaults. Rename them to anything — the Architect agent handles it during setup.

---

## How It Works Under the Hood

Three Man Team uses opencode's agent system and config deep-merge:

- **Architect** is a `primary` agent — you interact with it directly in your opencode session (press Tab to switch to it).
- **Builder** and **Reviewer** are `subagent` agents — Architect delegates to them via the Task tool when work needs building or reviewing.
- All inter-agent communication happens through files in the `handoff/` directory — not through conversation.
- **Global config** (`~/.config/opencode/`) defines the agents and skill — available in every project.
- **Project config** (`opencode.json` in project root) sets `default_agent`, project-specific `instructions`, and optional model overrides — opencode deep-merges project over global.
- Per-project install puts agents in `.opencode/agents/` instead — same files, just local to that project.

---

## Token Optimization

Every session starts with five rules baked into the token-optimizer skill:

```
Is this in a skill or memory?   → Trust it. Skip the file read.
Is this speculative?            → Kill the tool call.
Can calls run in parallel?      → Parallelize them.
Output > 20 lines you won't use → Route to subagent.
About to restate what user said → Delete it.
```

The token-optimizer skill ships with every install and auto-loads via opencode's skill system — no manual setup required.

For bash output compression on top of these rules, see [RTK](https://github.com/rtk-ai/rtk) —
a separate tool that compresses `find`, `ls`, `grep` output before it reaches the model's context.
Not required, but recommended for heavy opencode users. The combination of RTK (bash layer)
+ token-optimizer (behavior layer) is where real savings compound.

See `docs/token-optimization.md` for the full discipline.

---

## Auto-Update

The Architect checks the GitHub releases API at the start of every session. If a newer version is available, it tells you before doing anything else:

> "Three Man Team v2.0.1 is available — you're on v2.0.0. Before we get into today's work, let me walk you through what changed."

The Architect walks you through each change in plain language and guides you through any updates your project needs. You decide what to apply and when.

See [releases](https://github.com/alexzendermarunsai/three-man-team-opencode/releases) for what's changed.

---

## Templates

- `templates/global/` — **Reusable infrastructure.** Agent definitions (architect.md, builder.md, reviewer.md), token-optimizer skill, and agent config for opencode.json. Install at `~/.config/opencode/` for global use, or copy into a project's `.opencode/` for per-project use. Personas have `[CUSTOMIZE]` placeholders — fill them in or let the Architect agent handle it during first setup.
- `templates/project/` — **Project state.** handoff/ directory, project opencode.json (default_agent + instructions only), VERSION, new-setup.md, PROJECT.md. Copy into each project root. Agent definitions and skills come from global merge or per-project `.opencode/`.

The Architect handles persona customization during setup — just tell it your names.

---

## Credits

This is an [opencode](https://opencode.ai) port of the original [Three Man Team](https://github.com/russelleNVy/three-man-team) by **Russell Aaron**. The opencode port is maintained by **Alexzender Marunsai**. All design principles, personas, handoff formats, and workflow rules are unchanged from the original. See [`CREDITS.md`](CREDITS.md) for full attribution.

---

## License

MIT. Free forever. Copyright (c) 2026 Russell Aaron, Alexzender Marunsai.

---

## Built By

Russell Aaron — 20+ years building and supporting software the right way. He built this team
in production shipping a real SaaS platform. It works because it was used before it was
published, fine tuned, and will continue to get better over time as AI models and tools evolve.

Opencode port by [Alexzender Marunsai](https://github.com/alexzendermarunsai).