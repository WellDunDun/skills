# Agent Prompt Loop Reference

Use this reference when designing a loop that lets agents generate their own next prompts.

## Loop patterns

### Single-agent continuation

Use when one agent can safely carry a task across multiple turns.

Required artifacts:

- Start prompt.
- State log path.
- Continue prompt.
- Stop conditions.
- Final response checklist.

Good for: focused implementation tasks, small bug fixes, docs work, or repeated validation tasks.

### Driver-reviewer

Use when quality matters and a cheap independent review loop catches mistakes.

Flow:

1. Driver reads task, implements, and runs targeted checks.
2. Driver writes a review prompt with changed files, diff summary, tests, and risks.
3. Reviewer critiques for bugs, missing tests, and unclear assumptions.
4. Driver fixes actionable findings.
5. Verifier runs final commands and evidence capture.

Good for: PR-ready implementation, risky refactors, UI work, and build/test loop setup.

### Research-plan-build

Use when the agent should avoid coding before understanding the domain.

Flow:

1. Researcher gathers repo context and constraints.
2. Planner writes a concrete implementation plan and verification plan.
3. Builder executes the plan.
4. Reviewer verifies plan fit and result quality.

Good for: unfamiliar codebases, architecture changes, integrations, and migrations.

### Issue conveyor

Use when the user wants agents to keep pulling work without repeated prompting.

Flow:

1. Agent selects a ready issue by documented priority rules.
2. Agent confirms prerequisites and branch state.
3. Agent implements the smallest complete slice.
4. Agent verifies, writes evidence, and updates issue state.
5. Agent writes the next issue prompt or selects the next ready issue.

Good for: backlogs, triage queues, issue-to-PR workflows, and repetitive maintenance.

### PR hardening

Use when a PR needs iterative repair without user steering.

Flow:

1. Agent reads unresolved comments, CI failures, evidence gaps, and diff.
2. Agent ranks blockers by severity.
3. Agent fixes one coherent batch.
4. Agent reruns checks and updates the PR evidence section.
5. Agent writes the next repair prompt if work remains.

Good for: CI fixes, review comment resolution, and PR polish.

## Prompt artifact locations

Prefer existing repo conventions. If none exist, use one of:

- `AGENTS.md` for repo-wide loop rules.
- `skills/<project-loop>/SKILL.md` for reusable loop behavior.
- `.context/agent-loop/` for local generated prompts, handoffs, state, and evidence.
- Issue or PR comments when the loop is tied to GitHub work.

Suggested local paths:

```text
.context/agent-loop/
  state.md
  next-prompt.md
  review-prompt.md
  verify-prompt.md
  handoff.md
  blockers.md
```

## Prompt design rules

- Make prompts self-contained: include task, repo path, branch, files changed, commands run, failures, and next objective.
- Separate driver prompts from reviewer prompts so the reviewer does not inherit the driver's assumptions.
- Include explicit stop conditions in every loop.
- Include a verification gate before any final handoff.
- Include evidence requirements for UI, simulator, media, animation, or user-visible flows.
- Prefer one next action per prompt when work is risky; batch routine maintenance only when checks are cheap.

## Escalation rules

Agents should keep moving without user input unless:

- A product or design decision is genuinely ambiguous.
- Credentials, accounts, paid services, or secrets are missing.
- The next action is destructive or changes published state.
- The same blocker repeats after three serious attempts.
- Verification cannot be run and no narrower useful check exists.

Escalation questions must be specific and short. Include what was tried, the exact blocker, and the recommended option.

## Integration with build and verify loops

Every implementation loop should reference the project's build and verify loop. If none exists, run `setup-build-verify-loop` before creating a self-driving prompt loop.

Capture:

- Quick loop command.
- Targeted test command.
- Full loop command.
- Runtime smoke check.
- Evidence path and PR evidence requirement.
- Required supporting skills such as `serve-sim` for mobile simulator projects.
