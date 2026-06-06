# Starting a Three Man Team Session

## First-time setup

1. Run `opencode` in your project folder
2. Switch to the architect agent (press Tab)
3. Say: `Read new-setup.md.`

Arch will introduce the team, handle your project context file, and give you the prompt to use every session going forward.

---

## Architect Session (every session after setup)

1. Run `opencode` in your project folder
2. Switch to the architect agent (press Tab)
3. Say: `Read handoff/SESSION-CHECKPOINT.md (or handoff/BUILD-LOG.md if no checkpoint), then tell me where we are.`

## Builder (Architect delegates via Task tool)

When work is ready to build, Architect uses the Task tool to delegate to the builder subagent. Builder reads handoff/ARCHITECT-BRIEF.md and builds exactly what the brief says.

## Reviewer (Architect delegates via Task tool)

When Builder signals done, Architect uses the Task tool to delegate to the reviewer subagent. Reviewer reads handoff/REVIEW-REQUEST.md and the specific files Builder listed, then writes findings to handoff/REVIEW-FEEDBACK.md.

## Resuming After a Break

If handoff/SESSION-CHECKPOINT.md exists and is recent, use the resume prompt inside it.

Otherwise:

1. Switch to the architect agent
2. Say: `Read handoff/BUILD-LOG.md, then tell me where the project stands and what is next.`

## Tips

- First session always uses new-setup.md. Every session after uses the architect agent directly.
- Let Architect report status before giving any instructions.
- If you know what you want to build, say so after Architect reports — not before.
- Keep the Architect session focused on planning and diagnosis. Build and review happen in subagents.