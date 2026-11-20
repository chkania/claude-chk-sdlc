---
name: retro
description: Retrospective of the agent chain – analyzes the run log and proposes at most three improvements.
disable-model-invocation: true
---

1. Read `.claude/feature-profile.md` (path of the run log). Start the agent `chk-sdlc:process-retro` and tell it the plugin path: `${CLAUDE_PLUGIN_ROOT}` (there `agents/` and `skills/feature/SKILL.md` lie, which it may read).
2. Present the user with the proposals – for each: pattern, classification (plugin or project), file and change – and ask via AskUserQuestion (multi-select) which to adopt.
3. Apply only the chosen proposals with Edit:
   - **Project** (profile, checklist, run log): directly in the project.
   - **Plugin:** The installed plugin copy is a cache and gets overwritten – do not change it. Ask for the path of the local clone of `claude-chk-sdlc` (default `~/Documents/Development/claude-chk-sdlc`), change it there, and remind about the version increase in `plugin.json`, commit/push and `claude plugin update`.
   If a change conflicts with `CLAUDE.md` or another agent file, say so instead of applying it.
4. Append the line "Retro <date>: …" to the run log, supplemented by the adopted proposals.
5. Do not commit; name the changed files to the user.
