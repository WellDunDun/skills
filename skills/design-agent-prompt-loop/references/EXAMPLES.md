# Agent Prompt Loop Examples

## AGENTS.md loop section

```md
## Agent Prompt Loop

Goal: agents should complete ready implementation tasks with minimal user prompting.

Start:
- Read the issue or task.
- Read AGENTS.md and relevant project skills.
- Confirm the build and verify loop before editing.
- Create or update .context/agent-loop/state.md.

Continue:
- Read .context/agent-loop/state.md and .context/agent-loop/next-prompt.md.
- Complete the next smallest safe action.
- Run the relevant quick or targeted check.
- Update state.md with decisions, files changed, commands run, and blockers.
- Write the next prompt to .context/agent-loop/next-prompt.md.

Review:
- When implementation is coherent, write .context/agent-loop/review-prompt.md for an independent reviewer.
- The reviewer must prioritize bugs, missing tests, regressions, and evidence gaps.

Stop:
- Stop only when done, blocked by missing access, facing a destructive action, or needing a product decision.
- Ask the user one specific question with a recommended path.
```

## Start prompt template

```md
You are the driver agent for this repo.

Task: <task or issue link>
Repo: <absolute path>
Branch: <branch>

Instructions:
1. Read AGENTS.md and relevant skills.
2. Confirm the build and verify loop.
3. Inspect the task and related files.
4. Implement the smallest complete slice.
5. Run targeted verification.
6. Update .context/agent-loop/state.md.
7. Write the next prompt to .context/agent-loop/next-prompt.md unless the task is done.

Stop only for missing access, destructive actions, repeated blockers, or product decisions.
```

## Continue prompt template

```md
Continue the current agent loop.

Read:
- .context/agent-loop/state.md
- .context/agent-loop/next-prompt.md
- AGENTS.md

Then execute the next action, run the relevant check, update state.md, and write the next prompt. Do not ask the user for routine next steps already covered by the loop.
```

## Review prompt template

```md
You are an independent reviewer.

Review this work for bugs, behavioral regressions, missing tests, and evidence gaps.

Context:
- Task: <task>
- Changed files: <files>
- Diff summary: <summary>
- Commands run: <commands and results>
- Evidence: <paths or links>

Output findings first, ordered by severity. Include file and line references where possible. Do not rewrite the implementation unless asked.
```

## Verify prompt template

```md
You are the verifier.

Prove the current implementation is ready for handoff.

Run or explain:
- Quick loop: <command>
- Targeted tests: <command>
- Full loop: <command or reason skipped>
- Runtime smoke: <browser/simulator/desktop/CLI check>
- Evidence: <screenshots/videos/GIFs/logs>

Update .context/agent-loop/state.md with results and write final handoff notes.
```

## Handoff template

```md
# Agent Handoff

Task: <task>
Current status: <done/partial/blocked>

Changed files:
- <file>: <why>

Commands run:
- <command>: <result>

Evidence:
- <artifact or link>

Decisions made:
- <decision>

Remaining work:
- <next action>

Next prompt:
<self-contained prompt for the next agent>
```

## Failure recovery prompt

```md
The loop is blocked.

Summarize:
- Exact command or action that failed.
- First actionable error.
- What has already been tried.
- Whether this appears to be missing setup, missing credentials, repo state, external service, or a product decision.
- One recommended next step.

If the blocker can be worked around with a narrower verification command, do that and continue. Otherwise ask the user one specific question.
```
