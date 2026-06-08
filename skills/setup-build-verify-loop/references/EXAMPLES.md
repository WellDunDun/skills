# Build And Verify Examples

## AGENTS.md section

Use this when the repo needs global agent instructions.

```md
## Build And Verify

Project stack: <frameworks, language versions, package manager>.

Setup:
- <install command>
- Platform prerequisites: <browser/simulator/desktop runtime/services, or none>
- Required services: <database/cache/emulator/none>
- Required env files or secrets: <.env.example, secret names, or none>

Quick loop:
- <fast lint/typecheck/test command>
- Use after small code changes.

Targeted tests:
- <command pattern for one test file>
- <command pattern for one test name or package>

Full loop:
- <full test/build command>
- Run before PR-ready handoff or after shared behavior changes.

Runtime loop:
- Start: <dev server/simulator/desktop/service command>
- URL/device/window/port: <target>
- Platform prerequisites: <browser/simulator/device/desktop runtime requirements>
- Health check: <curl/browser/simulator/desktop smoke command>

Evidence:
- User-visible UI changes require screenshots for changed static states.
- Interactions, navigation, animations, media, gestures, loading, and error states require short video plus GIF.
- Save local evidence under .context/evidence/ unless a repo-specific path exists.
- PR-ready work must embed or link accessible evidence; local paths alone are not enough.

Reporting:
- Final responses must list commands run and results.
- If a documented command was not run, state why.
- If a command fails from existing repo state, include the first actionable error line.
```

## Project verification skill

Use this when the repo needs a reusable skill at `skills/<project>-verify/SKILL.md`.

```md
---
name: <project>-verify
description: Builds, runs, tests, and captures evidence for <project>. Use when implementing, fixing, reviewing, or preparing PR-ready work in this repository.
---

# <Project> Verify

Use this skill before and after code changes in this repository.

## Quick start

1. Read AGENTS.md and any task-specific instructions.
2. Run the quick loop before broad edits when the repo state is uncertain.
3. After changes, run the narrowest targeted check that covers the changed behavior.
4. Before final handoff, run the full loop or document why it could not be run.
5. For user-visible work, capture evidence and report artifact paths.

## Commands

Setup:
- <install/bootstrap command>

Run app:
- <runtime command>
- <URL/device/desktop window/port>
- Platform prerequisites: <browser/simulator/desktop runtime/services>

Quick check:
- <fast verification command>

Targeted tests:
- <targeted command examples>

Full check:
- <full verification command>

## Evidence

- Screenshots: .context/evidence/screenshots/
- Videos: .context/evidence/videos/
- GIFs: .context/evidence/gifs/
- Notes or logs: .context/evidence/notes/

Use clear timestamped names: <feature>-<scenario>-YYYYMMDD-HHMMSS.<ext>.

## Known issues

- <pre-existing command failure, missing service, flaky test, or none>

## Final response

Report commands run, results, evidence artifacts, and commands skipped with reasons.
```

## Final response checklist

```text
Build/verify loop created:
- Files changed: <paths>
- Verified commands: <command -> result>
- Documented but unverified commands: <command -> reason>
- Evidence policy: <added/referenced/not applicable>
- Remaining blockers: <none or concise list>
```

## PR evidence section

```md
## Evidence

Checks:
- <command>: <pass/fail/blocked>

Artifacts:
- GIF: <uploaded or attached URL>
- Video: <uploaded or attached URL>
- Screenshots: <uploaded or attached URLs>

Coverage:
- <scenario and states proven>

Caveats:
- <known missing state, skipped command, or none>
```
