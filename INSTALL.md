# Install Three Man Team

Three Man Team runs inside [opencode](https://opencode.ai) — the open source AI coding agent.
Make sure opencode is installed first: [opencode.ai/install](https://opencode.ai/install)

---

## Per-project install (recommended)

One project, one install. Clone directly into your project folder.

```bash
cd /path/to/your/project
git clone https://github.com/russelleNVy/three-man-team.git .opencode/skills/three-man-team
cd .opencode/skills/three-man-team && ./setup
```

Setup will give you the exact commands to copy the template files into your project
and tell you how to start your first session in opencode.

---

## Global install (all projects)

Install once, use in any project.

```bash
git clone https://github.com/russelleNVy/three-man-team.git ~/.config/opencode/skills/three-man-team
cd ~/.config/opencode/skills/three-man-team && ./setup
```

Then for each project you want to use Three Man Team on:

```bash
cp -r ~/.config/opencode/skills/three-man-team/templates/project-folder/. /path/to/your/project/
cd /path/to/your/project
opencode
```

Switch to the architect agent (press Tab) and say: `Read new-setup.md.`

---

## What Gets Installed

The template copies these files into your project:

- `.opencode/agents/architect.md` — Architect agent definition (primary agent)
- `.opencode/agents/builder.md` — Builder agent definition (subagent)
- `.opencode/agents/reviewer.md` — Reviewer agent definition (subagent)
- `.opencode/skills/token-optimization/SKILL.md` — Token discipline skill
- `opencode.json` — Project config (default_agent, agent definitions, skill paths)
- `handoff/` — Inter-agent communication files (5 files)
- `VERSION` — Current version tracker (for auto-update)
- `new-setup.md` — First-time setup instructions

---

## Starting a Session

After setup is complete:

1. Run `opencode` in your project folder
2. Switch to the architect agent (press Tab)
3. Say: `Read handoff/SESSION-CHECKPOINT.md (or handoff/BUILD-LOG.md if no checkpoint), then tell me where we are.`