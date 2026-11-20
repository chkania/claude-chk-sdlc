# claude-chk-sdlc

Marketplace with the Claude Code plugin **chk-sdlc**: an agent chain for features.

```
/chk-sdlc:feature <description>    Requirement → review → approval → implementation → code review → QA
/chk-sdlc:retro                    Retrospective over the run log
```

| Agent | Model | Task |
|---|---|---|
| `requirements-author` | Haiku | formulates the requirement, asks clarifying questions |
| `requirements-reviewer` | Opus | checks against the existing project and guardrails |
| `implementer` | Haiku | code, tests, docs |
| `code-reviewer` | Sonnet | diff review (correct, performant, secure, sensible) |
| `qa` | Sonnet | coverage, build, project-specific criteria, tests ↔ requirement |
| `process-retro` | Sonnet | analyzes the run log |

The user approves twice (short summary of the requirement, reviewed requirement) before implementation starts; push and PR only on request.

## Using it in a project

`.claude/settings.json` of the project:

```json
{
  "extraKnownMarketplaces": {
    "claude-chk-sdlc": { "source": { "source": "github", "repo": "chkania/claude-chk-sdlc" } }
  },
  "enabledPlugins": { "chk-sdlc@claude-chk-sdlc": true }
}
```

When you open the project, Claude Code asks whether to trust the marketplace. Then create `.claude/feature-profile.md` – format and template in [docs/PROFILE.md](docs/PROFILE.md).

Manually: `claude plugin marketplace add chkania/claude-chk-sdlc` and `claude plugin install chk-sdlc@claude-chk-sdlc --scope project`.

## Maintenance

- Make changes to agents/skills here, increase `version` in `plugins/chk-sdlc/.claude-plugin/plugin.json`, push.
- In the projects: `claude plugin update chk-sdlc@claude-chk-sdlc` (auto-update is off by default for your own marketplaces) and `/reload-plugins`.
- Test locally: `claude --plugin-dir ./plugins/chk-sdlc`.
- Validate: `claude plugin validate .`
- Project-specific content does **not** belong here, but in `.claude/feature-profile.md` of the project.
