# Build And Verify Reference

Use this file for detailed discovery, platform prerequisites, and failure handling.

## Discovery checklist

Run targeted discovery before opening many files:

```sh
rg --files -g 'AGENTS.md' -g 'CLAUDE.md' -g 'package.json' -g 'pyproject.toml' -g 'Cargo.toml' -g 'go.mod' -g 'pubspec.yaml' -g 'Makefile' -g 'justfile' -g 'Taskfile.yml' -g '.github/workflows/*' -g 'README*' -g 'CONTRIBUTING*'
```

Read existing files in this order:

1. Agent instructions and contributor docs.
2. CI workflows, because they usually encode the trusted verification path.
3. Package manifests and task runner files.
4. README setup and development sections.
5. Existing project-local skills or scripts.

Capture exact commands for:

- Dependency install or bootstrap.
- Environment or secrets setup.
- Local app startup, including default ports.
- Lint, format check, typecheck, and static analysis.
- Unit, integration, and end-to-end tests.
- Production build.
- Storybook, component preview, simulator, desktop runtime, or local sandbox startup.
- Database migration, seed, reset, service startup, and cleanup.

Package manager signals:

- Prefer the lockfile: `pnpm-lock.yaml` means pnpm, `yarn.lock` means yarn, `package-lock.json` means npm, `bun.lockb` or `bun.lock` means bun.
- Prefer workspace scripts over package-level guesses in monorepos.
- If CI conflicts with the lockfile, document the conflict and prefer CI until the user confirms otherwise.

## Platform prerequisites

Use this section when the project has a user-visible runtime surface.

### Web apps

Capture:

- Package manager and install command.
- Dev server command, default port, and alternate port behavior.
- Production build command.
- Browser automation path: Playwright, Cypress, app-specific test runner, or manual browser verification.
- Required env files, local services, auth fixtures, seed data, and test accounts.
- Screenshot and video capture path for changed routes or responsive states.

Runtime loop shape:

```md
Runtime loop:
- Start: <dev server command>
- URL: http://localhost:<port>/<route>
- Smoke: <curl health route or browser route load>
- Browser verification: <Playwright/Cypress/manual steps>
```

### Mobile apps

Capture:

- Platform: iOS, Android, Flutter, React Native, native, or hybrid.
- Install/bootstrap command and SDK manager requirements.
- Simulator/emulator list command and preferred target device.
- App run command for simulator/emulator.
- Unit/widget tests and integration/e2e tests.
- Required agent skill: `serve-sim` for simulator-driven mobile verification.
- Runtime inspection path: serve-sim, platform tooling, Flutter VM service, React Native devtools, or app-specific test runner.
- Recording and screenshot commands.

For simulator-driven mobile projects, add the `serve-sim` skill as a prerequisite and default to it when available.

```md
Mobile runtime loop:
- List devices: npx --yes serve-sim --list -q
- Start app: <project-specific simulator run command>
- Inspect UI: use serve-sim /ax or equivalent accessibility tree before tapping.
- Interact: prefer accessibility-derived targets; do not guess coordinates when the tree exposes the element.
- Evidence: record starting state, action, result state, and back/return path when relevant.
```

Document required simulator/emulator runtime version, device name, required agent skills such as `serve-sim`, diagnostic overlay rules, backend/tunnel/proxy needs, fixture login state, known startup issues, and reset commands.

### Desktop apps

Capture:

- Platform: Electron, Tauri, native macOS, Windows, Linux, Java, .NET, Qt, or other.
- Install/bootstrap command.
- Dev run command and packaged build command.
- Required OS permissions, signing/notarization requirements, helper services, and local data directories.
- UI automation path: Playwright Electron, webdriver, AppleScript, accessibility automation, or manual steps.
- Screenshot or screen recording path.

```md
Desktop runtime loop:
- Start: <desktop dev command>
- Smoke: <window appears, health command, or log line>
- Verify: <automation command or manual scenario>
- Evidence: <screenshot/video path>
```

### CLI and service UIs

Capture sample command with fixture input, expected output shape, exit code, local service startup, health check, log locations, and cleanup commands for generated files or local state.

## Failure handling

If a command fails while setting up the loop, capture:

- Exact command.
- Exit status.
- First actionable error line.
- Whether the failure appears caused by missing setup, missing secret, external service, or existing repo state.
- Whether a narrower command still gives useful confidence.

Do not normalize failures away. A loop that documents real blockers is more useful than one that claims a clean path that does not exist.

## Evidence defaults

- Static UI change: screenshot changed states.
- Interaction, navigation, animation, media, gesture, loading, or error state: video plus GIF.
- Simulator or desktop flows: show starting state, action, result, and return path when relevant.
- PR-ready work: upload or attach evidence where reviewers can access it; local paths alone are not enough.
