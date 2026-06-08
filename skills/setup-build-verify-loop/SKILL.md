---
name: setup-build-verify-loop
description: Sets up project-specific build, test, runtime, and evidence verification loops for AI agents. Use when onboarding a repo, documenting build/test commands, creating AGENTS.md instructions, preparing a project verification skill, or turning an ad hoc workflow into a repeatable build-and-verify loop.
---

# Setup Build Verify Loop

Create a verified loop that tells future agents exactly how to build, run, test, and prove work in a project.

## Quick start

1. Discover the project shape from source-of-truth files before writing instructions.
2. Extract commands from manifests, CI, task runners, docs, and existing agent guidance.
3. Run representative commands before documenting them as verified.
4. Write the loop into the narrowest durable surface: `AGENTS.md`, a project skill under `skills/`, or an existing project convention.
5. Include runtime and evidence requirements for user-visible work.
6. Final response must list changed files, verified commands, unverified commands, evidence policy, and blockers.

## Workflow

### 1. Discover

Read the relevant files first. Prefer `rg --files` and targeted reads.

Look for:

- Agent guidance: `AGENTS.md`, `CLAUDE.md`, `skills/**/SKILL.md`, `.cursor/rules`.
- Build metadata: package manifests, lockfiles, language config, Dockerfiles, and build files.
- Task runners: `Makefile`, `justfile`, `Taskfile.yml`, `turbo.json`, `nx.json`, `moon.yml`, `mise.toml`.
- CI truth: `.github/workflows/*`, `.gitlab-ci.yml`, CircleCI, Bitrise, Fastlane, deployment configs.
- Runtime surfaces: web routes, mobile simulators, desktop apps, CLIs, workers, APIs, seed data, local services.

See [REFERENCE.md](references/REFERENCE.md) for discovery commands, platform prerequisites, and failure handling.

### 2. Classify checks

Define commands by speed and purpose:

- Setup or doctor: proves dependencies, required services, env files, and tool versions.
- Quick loop: fastest useful check after small edits.
- Targeted tests: narrow command for one package, file, behavior, or scenario.
- Full loop: PR-ready confidence, usually full tests plus production build.
- Runtime loop: starts the app, simulator, desktop app, CLI, or service and proves behavior.
- Evidence loop: captures screenshots, videos, GIFs, logs, or test output reviewers can inspect.

### 3. Prove before persisting

Document commands according to what happened:

- Passed locally: record the exact command as verified.
- Failed from current repo state: record the command, first actionable error, and why it appears pre-existing.
- Failed from missing dependency or secret: record prerequisites and mark unverified.
- Not run because destructive, too slow, or external: mark unverified and explain why.

Never hide failures by omitting the command. Future agents need the real operating envelope.

### 4. Write the loop

Keep the output command-first. Include:

- Project stack and package manager.
- Install/bootstrap command.
- Development server, simulator, desktop, CLI, or service startup command.
- Quick checks, targeted tests, and full checks.
- Runtime smoke checks and visual evidence requirements.
- Known prerequisites, services, secrets, ports, and common failure modes.
- Final response checklist future agents must report.

Use [EXAMPLES.md](references/EXAMPLES.md) for `AGENTS.md`, project skill, final response, and PR evidence templates.

## Evidence policy

For user-visible work, require reviewable evidence in addition to tests.

- UI-only visual change: screenshots for changed states.
- Interaction, navigation, animation, media, gestures, loading, or error states: short video plus GIF; add screenshots when visual states changed.
- Backend-only work: test output is enough unless it changes visible behavior.
- Store local artifacts under `.context/evidence/` or the repo's established equivalent.
- PR-ready work must embed or link evidence where reviewers can access it; local paths alone are not enough.

Default to `serve-sim` for mobile simulator control when available. See [REFERENCE.md](references/REFERENCE.md) for platform-specific prerequisites.

## Guardrails

- Do not invent commands from framework habits when the repo has explicit scripts or CI.
- Do not add new task runner dependencies just to make the loop neat.
- Do not claim tests pass unless you ran the relevant command.
- Keep project-specific skills focused on that project; keep this setup skill generic.
