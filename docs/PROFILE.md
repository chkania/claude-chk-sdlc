# Project profile `.claude/feature-profile.md`

The plugin's agents are project-neutral. Everything project-specific lives in this file in the project's repo. In addition, the agents expect the following in the project:

- **Requirements directory** (default `docs/anforderungen/`) with `README.md` (index, maintenance rules), `produktbeschreibung.md` (product description: scope and guardrails against invented requirements), `annahmen.md` (assumptions), `aenderungsprotokoll.md` (change log), one file per requirement.
- **Checklist** (default `.claude/requirements-checklist.md`): format of the requirement and numbered check items.
- **Run log** (default `.claude/feature-log.md`): a short section per run.
- `CLAUDE.md` (conventions) and `CONTRIBUTING.md` (commit rules).

## Template

```markdown
# Feature profile

## Project
<name and purpose in one sentence>

## Paths
- Requirements: docs/anforderungen/
- Checklist: .claude/requirements-checklist.md
- Run log: .claude/feature-log.md
- Default branch: main

## Commands
- Targeted tests: `<e.g. npx vitest run --reporter=dot <files>>`
- Full test suite: `<npm test -- --reporter=dot>`
- Tests with coverage: `<npm run test:coverage -- --reporter=dot>`
- Build/type check: `<npm run build>`
- Baseline measurement (optional): `<command and what is written to a file, e.g. file sizes from dist/assets>`

## Cleanup after test runs
<e.g. `git checkout -- .vitest/json/output.json`; otherwise "none">

## Documentation duties (implementer)
- <e.g. new field → user manual>

## Review focus (code review)
- Correct: <domain specifics, sign rules, data formats>
- Performant: <e.g. only lazy-load library X>
- Secure: <e.g. escape output, CSP>
- Structure: <where things belong>

## QA criteria (in addition to tests, coverage, build, test domain correctness)
- Minimum coverage: <source/values>
- <performance/size: comparison with baseline, threshold>
- <accessibility/browser check: condition and procedure>
- <documentation/interface contract>

## Special notes
<anything else, e.g. notes for other users of the project>
```
