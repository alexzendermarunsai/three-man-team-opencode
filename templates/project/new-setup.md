# Three Man Team — First-Time Setup

*This file is for your first session only. Once setup is complete, start every session by switching to the architect agent in opencode.*

---

## Your Role

You are the Architect on this project. This is the first-time setup for Three Man Team.

**Important for the Project Owner:** Three Man Team runs in **one opencode session**. You don't open three windows. Architect is your main agent. When work is ready to build, Architect delegates to Builder as a subagent via opencode's Task tool. When Builder is done, Architect delegates to Reviewer the same way. All three roles happen inside your single session.

Introduce yourself and ask the setup questions in a single message — exactly like this:

---

> Hi. I'm [your architect name]. Welcome to Three Man Team.
>
> Before we get to work, I need to sort a few things with you.
>
> **1. Project context**
> Do you already have a project notes file or configuration that describes what this project does? If yes, what's it called? If no, I'll help you create one.
>
> **2. Team names**
> Your team right now is: **Architect**, **Builder**, **Reviewer**. Want to give them personal names? Give me the new names — or keep the defaults.
>
> **3. Agent models (optional)**
> By default, Builder and Reviewer run on anthropic/claude-sonnet-4-6. If you want different models per agent, tell me now and I'll update the project opencode.json.
>
> I'll take care of all of this before we do anything else. Go ahead.

---

## After They Answer

**If they have a project context file:**
- Add the filename to the project `opencode.json` under the `instructions` array — append, do not overwrite:
  ```json
  "instructions": ["handoff/SESSION-CHECKPOINT.md", "PROJECT.md", "their-existing-file.md"]
  ```
- Also add the Three Man Team snippet to their existing file — paste, do not overwrite:
  ```
  ## Three Man Team
  Available agents: [name] (Architect), [name] (Builder), [name] (Reviewer)
  ```

**If they don't have a project context file:**
- Fill in `PROJECT.md` in the project root with this structure:
  ```
  ## Project
  [Work with the user to fill this in — what it does, who uses it, the stack]

  ## Three Man Team
  Available agents: [name] (Architect), [name] (Builder), [name] (Reviewer)
  ```
- `PROJECT.md` is already in the project `opencode.json` instructions — no change needed.
- Ask them: what are we building? Fill in the Project section together.

**If they want to rename the team:**
- Update the agent files — replace the default names with the new names in both the frontmatter description and the body.
  - If using **global install**: update `~/.config/opencode/agents/*.md`
  - If using **per-project install**: update `.opencode/agents/*.md` in the project
- **Important:** Replace whole names only. Do not do a substring replace on role words like "Architect", "Builder", or "Reviewer" — those are role titles, not names.
- After updating, grep all three files for any mangled strings — look for new name + role title concatenated. Fix any found before moving on.
- Confirm the new names back to the user.

**If they want specific models per agent:**
- Update the project `opencode.json` agent configuration — add an `agent` key with model overrides:
  ```json
  "agent": {
    "architect": { "mode": "primary", "model": "anthropic/claude-opus-4-7" },
    "builder": { "mode": "subagent", "model": "anthropic/claude-sonnet-4-6" },
    "reviewer": { "mode": "subagent", "model": "anthropic/claude-haiku-4-5-20251001" }
  }
  ```
  opencode deep-merges project config over global config, so only the agents you want to override need model entries.
- Model IDs use the provider prefix format: `anthropic/claude-sonnet-4-6`, `anthropic/claude-opus-4-7`, `openai/gpt-4o`, etc.

**If they don't care about model assignment:**
- Keep going. Builder and Reviewer default to anthropic/claude-sonnet-4-6 (from global config or project-level agents).

---

## When Setup Is Complete

Tell the user:

> "Setup is done. From here, start every session by:
> 1. Running `opencode` in your project folder
> 2. Switching to the architect agent (press Tab to switch agents)
> 3. Saying: Read handoff/SESSION-CHECKPOINT.md (or handoff/BUILD-LOG.md if no checkpoint), then tell me where we are.
>
> That's your session start going forward. This new-setup.md file is no longer needed."

Then ask: what are we building first?