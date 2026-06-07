# Credits

This project is a fork of [Three Man Team](https://github.com/russelleNVy/three-man-team) by **Russell Aaron**.

## Original Project

- **Name:** Three Man Team
- **Author:** [Russell Aaron](https://russellenvy.com)
- **Repository:** [github.com/russelleNVy/three-man-team](https://github.com/russelleNVy/three-man-team)
- **License:** MIT (Copyright (c) 2026 Russell Aaron)
- **Original version:** v1.2.5 (Claude Code)

## This Fork

- **Name:** Three Man Team — opencode port
- **Maintainer:** [Alexzender Marunsai](https://github.com/alexzendermarunsai)
- **Repository:** [github.com/alexzendermarunsai/three-man-team-opencode](https://github.com/alexzendermarunsai/three-man-team-opencode)
- **License:** MIT (Copyright (c) 2026 Russell Aaron, Alexzender Marunsai)
- **Fork version:** v2.0.0 (opencode)

## What Changed

The v2.0.0 fork ported the entire framework from Claude Code to opencode:

- Agent definitions moved from root-level markdown files to `.opencode/agents/*.md` or `~/.config/opencode/agents/*.md` with YAML frontmatter
- Config replaced: `CLAUDE.md` removed, replaced by two `opencode.json` files — global (agent definitions + skills) and project (default_agent + instructions)
- Template architecture redesigned: `templates/global/` (agents, skills, config for `~/.config/opencode/`) and `templates/project/` (handoff state, project config, VERSION) — leverages opencode's deep-merge system
- `templates/generic/` deleted — replaced by `templates/global/` which actually uses opencode's global config system
- `templates/project-folder/` renamed to `templates/project/` — now only contains project-scoped files (no agents or skills)
- Architect is a `primary` agent, Builder and Reviewer are `subagent` agents
- Delegation uses opencode's Task tool instead of Claude Code's Agent tool
- Token-optimizer skill moved to `.opencode/skills/token-optimization/SKILL.md` or `~/.config/opencode/skills/token-optimization/SKILL.md`
- Model IDs use opencode's provider-prefix format (`anthropic/claude-sonnet-4-6`)
- Model overrides go in project-level `opencode.json` — deep-merges over global agent definitions
- Setup script rewritten with `project` and `global` modes
- Docs, landing page, and release JSON updated for opencode and deep-merge architecture

All design principles, personas, handoff file formats, the deploy gate, scope lock, and token discipline rules are unchanged from the original project.