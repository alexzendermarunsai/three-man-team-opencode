# Install Three Man Team

Three Man Team runs inside [opencode](https://opencode.ai) — the open source AI coding agent.
Make sure opencode is installed first: [opencode.ai/install](https://opencode.ai/install)

---

## Per-project install (recommended)

One project, one install. Clone directly into your project folder.

```bash
cd /path/to/your/project
git clone https://github.com/alexzendermarunsai/three-man-team-opencode.git .opencode/skills/three-man-team
cd .opencode/skills/three-man-team/templates/project && ./setup project
```

Setup will give you the exact commands to copy all files into your project — agent definitions, skills, handoff state, config, VERSION, and new-setup.md.

---

## Global install (all projects)

Install once — agents and skills are available in every opencode project automatically via deep-merge. You only copy project state files into each project.

```bash
git clone https://github.com/alexzendermarunsai/three-man-team-opencode.git ~/.config/opencode/skills/three-man-team
cd ~/.config/opencode/skills/three-man-team/templates/project && ./setup global
```

Setup installs:
- Agents → `~/.config/opencode/agents/` (architect.md, builder.md, reviewer.md)
- Skill → `~/.config/opencode/skills/token-optimization/SKILL.md`
- Config → merges agent definitions into `~/.config/opencode/opencode.json` (does NOT set `default_agent` or `instructions` globally)

Then for each project you want to use Three Man Team on:

```bash
cp -r ~/.config/opencode/skills/three-man-team/templates/project/. /path/to/your/project/
cd /path/to/your/project
opencode
```

opencode automatically deep-merges the global agents + skill into this project. No need to copy agent files per-project.

Switch to the architect agent (press Tab) and say: `Read new-setup.md.`

---

## What Gets Installed

### Global install

| File | Location | Purpose |
|---|---|---|
| `agents/architect.md` | `~/.config/opencode/agents/` | Architect agent (primary) — available in all projects |
| `agents/builder.md` | `~/.config/opencode/agents/` | Builder agent (subagent) — available in all projects |
| `agents/reviewer.md` | `~/.config/opencode/agents/` | Reviewer agent (subagent) — available in all projects |
| `skills/token-optimization/SKILL.md` | `~/.config/opencode/skills/` | Token discipline — available in all projects |
| agent definitions in opencode.json | `~/.config/opencode/opencode.json` | Agent modes + models (merged, NOT default_agent or instructions) |

### Per-project (each project)

| File | Location | Purpose |
|---|---|---|
| `opencode.json` | project root | `default_agent`, `instructions`, optional model overrides |
| `handoff/` (5 files) | project root | Inter-agent communication |
| `VERSION` | project root | Version tracker for auto-update |
| `new-setup.md` | project root | First-time setup instructions (delete after first session) |
| `PROJECT.md` | project root | Project context placeholder |

### Per-project install also adds

| File | Location | Purpose |
|---|---|---|
| `.opencode/agents/architect.md` | project `.opencode/` | Architect agent (overrides global if both exist) |
| `.opencode/agents/builder.md` | project `.opencode/` | Builder agent (overrides global if both exist) |
| `.opencode/agents/reviewer.md` | project `.opencode/` | Reviewer agent (overrides global if both exist) |
| `.opencode/skills/token-optimization/SKILL.md` | project `.opencode/` | Token discipline (overrides global if both exist) |

---

## Config Deep-Merge Behavior

opencode deep-merges global config (`~/.config/opencode/`) with project config:

| What | Global | Project | Result |
|---|---|---|---|
| Agent definitions | `~/.config/opencode/agents/*.md` | `.opencode/agents/*.md` | **Project overrides global** |
| Skill definitions | `~/.config/opencode/skills/` | `.opencode/skills/` | **Project overrides global** |
| `default_agent` | (not set) | `"architect"` | `"architect"` (project-level) |
| `instructions` | (not set) | `["handoff/...", "PROJECT.md"]` | Project-level (concatenated + deduplicated) |
| `agent.*.model` | `anthropic/claude-sonnet-4-6` | (override) | **Project overrides global** |

This means:
- Global install: agents available everywhere, project config only adds routing + state
- Per-project install: same agents + skills but local to that project
- Switching from per-project to global: delete project `.opencode/agents/` and `.opencode/skills/`, global merge takes over

---

## Starting a Session

After setup is complete:

1. Run `opencode` in your project folder
2. Switch to the architect agent (press Tab)
3. Say: `Read handoff/SESSION-CHECKPOINT.md (or handoff/BUILD-LOG.md if no checkpoint), then tell me where we are.`

That's your session start going forward.