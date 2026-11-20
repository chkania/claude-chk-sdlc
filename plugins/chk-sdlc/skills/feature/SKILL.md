---
name: feature
description: Agent chain for a new feature or change – requirement, review, user approval, implementation, code review, QA – with a project-specific profile.
disable-model-invocation: true
---

You control the agent chain for the user's input (arguments of this invocation). Subagents cannot start further agents; every handoff runs through you. After each stage, tell the user in one or two sentences where the chain stands.

The agents are `chk-sdlc:requirements-author`, `chk-sdlc:requirements-reviewer`, `chk-sdlc:implementer`, `chk-sdlc:code-reviewer`, `chk-sdlc:qa` (as `subagent_type`). They read the project profile `.claude/feature-profile.md` themselves.

**Keep handoffs lean:** Longer texts (requirement, reports) you save as a file in the scratchpad and give the agent the path plus base or interim state – it does not know this conversation. Agents with multiple passes you resume via SendMessage so they keep their context.

## 0. Profile

If `.claude/feature-profile.md` does not exist, stop and point to the template `PROFILE.md` in the plugin's repo ("Template" section). Read the profile and note the paths (requirements, run log, checklist) and the build/measurement command for the baseline measurement.

## 1. Branch and base

`git fetch origin`. If the working directory is clean, create a new branch `feat/<short-name>` from the default branch (`origin/main`, otherwise per profile). Otherwise ask the user whether the open changes belong to this feature.
- Note the commit as the **base** (`git rev-parse HEAD`).
- If the profile has a "Baseline measurement", run it and write the result to a file in the scratchpad (for QA).

## 2. Requirement – `requirements-author`

Start it with the user's input verbatim. On `STATUS: QUESTIONS`: ask the open points via AskUserQuestion and give the answers to the same agent via SendMessage. Repeat until `STATUS: DONE`. Note topic and answer of every user question for the run log.

### Interim approval by the user

Before the requirement goes to the reviewer, show the user the author's "Summary" (unchanged) and ask via AskUserQuestion: "Accept requirement as is?" (options: accept / changes needed).
- **Accepted:** continue with step 3.
- **Changes:** pass the user's wishes via SendMessage to the same `requirements-author`; repeat until `STATUS: DONE`, new summary, ask again.

Without agreement the requirement does not go to the reviewer.

## 3. Requirement review – `requirements-reviewer`

Hand over the finished requirement together with its "Notes for review" and the user's decisions. Handle questions as in step 2, then send back to the reviewer via SendMessage. The result is the **reviewed requirement**.

### Approval by the user

Save the reviewed requirement as a file in the scratchpad and give the user the full path (so they can read it there). Summarize it in a few lines: goal, IDs, core points, assumptions, what is explicitly not included. Ask via AskUserQuestion: "Approve requirement?" (options: approve / changes needed).
- **Approved:** continue with step 4.
- **Changes:** pass the user's wishes via SendMessage to the `requirements-reviewer` (for substantive rewording to the `requirements-author`), save the changed version as a file again and present it for approval again.

Without explicit approval, implementation does not start.

## 4. Implementation – `implementer`

Only after approval from step 3. Hand over the approved requirement. After each round of the implementer, create a local **interim state** (`git add -A && git commit -q -m "chore: wip"`) and note the commit. This way code review and QA receive only the delta in later passes.

## 5. Code review – `code-reviewer`

Hand over the reviewed requirement, the implementer's report and the base (first pass) or the last reviewed interim state (further passes).
- `STATUS: CHANGES` (high or medium findings): to the `implementer`, interim state, new code review on the delta. After two passes with still-high findings: stop and present the findings to the user.
- `STATUS: OK`: **collect** low findings and proceed to QA.

## 6. QA – `qa`

Hand over the reviewed requirement, the base, the file with the baseline measurement (if present) and, in further passes, the last reviewed interim state.
- `STATUS: DEFECTS`: defects to the `implementer`, interim state, then step 5 (delta) and 6 again. After three passes without approval: stop and present the defects to the user.
  - **Tests do not match the requirement:** If QA still objects to tests in the second pass, stop immediately. Present to the user the objected tests (`file:name`), the affected acceptance criterion and QA's reasoning, and ask via AskUserQuestion how to proceed (e.g. have the test adjusted, sharpen the requirement, accept the test as is, abort). Wait for the answer.
- `STATUS: APPROVED`: bundle the collected low findings (from code review and QA) once and send them to the `implementer`, without renewed review; its completion must report tests and build green.

## 7. After approval

1. **Run log:** append a section following the pattern in the run log (path in profile): stages with model, passes, tokens and duration from the completion reports; late-found findings with the stage that could have prevented them; user questions with **topic and answer** (not just the count); at most 15 lines.
2. Consolidate interim states: `git reset --soft <base>`, then one commit (including run log) following the project's commit rules (`CONTRIBUTING.md`, profile).
3. Ask the user whether to push now and create the PR (push only on their request). If yes: push, create PR with summary, result changes and QA table.

Final report to the user: reviewed requirement (IDs), passes per stage, QA result, commit or PR link. Recommend `/chk-sdlc:retro` if a stage in this run needed three or more passes or five runs have passed since the last retro in the run log.
