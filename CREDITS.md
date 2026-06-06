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

- Agent definitions moved from root-level markdown files to `.opencode/agent/*.md` with YAML frontmatter
- Config replaced: `CLAUDE.md` removed, replaced by `opencode.json` + `.opencode/` directory structure
- Architect is a `primary` agent, Builder and Reviewer are `subagent` agents
- Delegation uses opencode's Task tool instead of Claude Code's Agent tool
- Token-optimizer skill moved to `.opencode/skills/token-optimization/SKILL.md`
- Model IDs use opencode's provider-prefix format (`anthropic/claude-sonnet-4-6`)
- Setup scripts, docs, and landing page adapted for opencode

All design principles, personas, handoff file formats, the deploy gate, scope lock, and token discipline rules are unchanged from the original project.