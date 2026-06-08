---
name: design-agent-prompt-loop
description: Designs autonomous prompting loops that let coding agents drive their own next steps, handoffs, reviews, and verification prompts. Use when setting up agent workflows, reducing repeated user prompting, creating self-driving project instructions, designing prompt queues, or coordinating build, review, and handoff loops across agents.
---

# Design Agent Prompt Loop

Create a durable loop that prompts agents on the user's behalf so the user can delegate an outcome instead of repeatedly writing follow-up prompts.

## Quick start

1. Identify the project outcome the user wants agents to drive without repeated prompting.
2. Discover existing agent guidance, skills, issue trackers, verification commands, and review requirements.
3. Design a loop with triggers, agent roles, prompt cadence, stop conditions, and evidence requirements.
4. Write the loop into `AGENTS.md`, a project skill, issue templates, or a dedicated loop file the repo already uses.
5. Include prompts for start, continuation, review, handoff, and recovery from failure.
6. Final response must report where the loop lives, what it prompts agents to do, and how the user starts it.

## Loop shape

A useful agent prompt loop includes:

- Goal: the outcome agents should pursue.
- Inputs: issue, PRD, bug report, trace, design, branch, or repo context.
- Roles: driver, reviewer, verifier, researcher, designer, or release agent.
- Cadence: when the next prompt is generated and who receives it.
- Verification: commands, runtime checks, evidence, and review gates.
- Memory: where decisions, blockers, evidence, and next prompts are recorded.
- Stop conditions: done, blocked, unsafe, missing access, or needs user decision.
- Escalation: exact questions agents ask the user only when autonomous progress is no longer safe.

## Workflow

### 1. Discover the operating surface

Read project instructions and source-of-truth files before designing the loop:

- Agent guidance: `AGENTS.md`, `CLAUDE.md`, `skills/**/SKILL.md`, `.cursor/rules`.
- Planning artifacts: issues, PRDs, specs, TODO docs, design notes, traces, or roadmaps.
- Verification artifacts: build/test commands, CI, evidence rules, PR templates, release checks.
- Collaboration surfaces: GitHub issues/PRs, local markdown trackers, task boards, review comments.

Use `setup-build-verify-loop` first when the repo lacks a verified build/test/evidence loop.

### 2. Pick a loop pattern

Choose the smallest loop that removes repeated user prompting:

- Single-agent continuation: one agent works, records state, generates its next prompt, and continues.
- Driver-reviewer: a driver implements; a reviewer agent critiques; the driver fixes; verifier checks.
- Research-plan-build: one agent researches, one creates the plan, one executes, one verifies.
- Issue conveyor: agents pull ready issues, execute, verify, write handoff, then pick the next issue.
- PR hardening: agents inspect comments, CI, evidence gaps, and generate the next repair prompt.

See [REFERENCE.md](references/REFERENCE.md) for pattern details and failure handling.

### 3. Write prompts as artifacts

Do not leave the loop as prose only. Create durable prompt artifacts:

- Start prompt: how an agent begins from an issue, goal, or branch.
- Continue prompt: how an agent resumes from current state.
- Review prompt: how an independent agent critiques work.
- Verify prompt: how an agent proves behavior and evidence.
- Handoff prompt: what the next agent needs if the current agent stops.
- Recovery prompt: what to do after failed tests, missing access, or ambiguous requirements.

See [EXAMPLES.md](references/EXAMPLES.md) for pasteable templates.

### 4. Add guardrails

- Agents must use repo source-of-truth commands and skills, not guesses.
- Agents must record blockers with exact failing commands or missing decisions.
- Agents must not ask the user for routine next steps covered by the loop.
- Agents must ask the user only for product decisions, missing credentials, destructive actions, or repeated blockers.
- Every user-visible implementation loop must include verification evidence.

## Final response

Report:

- Files changed.
- Loop pattern chosen.
- How to start the loop.
- Where next prompts, handoffs, blockers, and evidence are recorded.
- Validation run, or why validation was not applicable.
