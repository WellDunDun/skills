---
name: design-agent-native-cli
description: Design and audit CLIs that agents can discover, call without prompts, and recover across retries and interrupted jobs. Use when building a CLI, reviewing agent usability, adding JSON output or machine-readable introspection, or improving idempotency, async execution, profiles, and artifact delivery.
---

# Design an agent-native CLI

Design command contracts and observable checks using the ten principles below. Apply them to the user's requested commands and workflows.

## Workflow

1. Read project instructions, command definitions, help, configuration, and existing tests. Identify intended agent tasks and compatibility commitments.
2. Trace a representative workflow through discovery, input, execution, output, and recovery. Include a failing invocation and a retry.
3. Map the ten principles below to affected commands. Mark conditional capabilities as not applicable with a reason, such as no remote jobs or repeated configuration.
4. For an audit, report evidence and prioritized fixes. For implementation, change shared command definitions or runtime helpers where they prevent repeated inconsistencies.
5. Verify the changed behavior through the executable. Use [verification.md](references/verification.md) to choose relevant checks and record anything untested.

Use [contracts.md](references/contracts.md) when specifying JSON, recovery, conditional writes, help, introspection, or delivery. Its commands and schemas are illustrative; inspect the actual executable before using them.

## Tier 1: reliable invocation

### 1. Run without interaction

Accept structured inputs through flags, stdin, or files. In a non-TTY session, either execute with explicit inputs and authorization flags or fail promptly with the exact missing requirement. Never wait for a prompt nobody can answer.

Where stdin fits the command, accept piped or redirected input without prompting. Verify stdin handling and chain machine-readable output into another command, including upstream failure and empty-result cases.

Provide a consistent `--no-input` mode where prompts exist. Use an established confirmation flag, such as `--force` for a destructive operation. A prompt bypass does not grant permission for unrelated actions.

### 2. Return structured output

Provide `--json` on data-returning commands. Keep results on stdout, diagnostics and progress on stderr, and suppress ANSI outside a TTY. In JSON mode, make errors parseable on stderr too.

Document stable response shapes and exit codes. Return resource or job identifiers in mutation responses. Never mix banners, spinners, or logs into a JSON document.

Report partial success with a nonzero exit, created IDs, completed and failed steps, and a recovery command. Mark uncertain side effects as unknown. Recovery must preserve completed work and avoid duplicate mutations.

### 3. Make errors teach recovery

Validate inputs before side effects. Name the rejected flag and value, enumerate supported enum values, and include a valid invocation. For large or dynamic sets, give a bounded sample and the discovery command.

Distinguish invalid input, authentication, missing resources, transient failures, and interrupted waits. Show useful diagnostics without exposing credentials or defaulting to a stack trace.

### 4. Make retries safe and mutations explicit

Use scoped idempotency keys or genuine domain keys for creates. Reusing a key with the same request returns the original operation; conflicting input gets a clear error. Identical content alone does not prove duplicate intent.

Provide `--dry-run` for consequential mutations and explicit confirmation flags for destructive actions. Preview targets and effects without performing the mutation. Preserve retry identity across submission, waiting, and collection.

For concurrent updates or deletes, use backend-enforced version or ETag preconditions where supported. Reject stale writes with a conflict and require rereading and reassessing the change. Never silently drop a precondition on retry; document any backend limitation.

### 5. Bound responses

Bound default list sizes, logs, and nested detail. Support filters, limits, and cursor pagination where relevant. Include truncation state, continuation information, and a concrete way to narrow the next request.

Keep help and introspection discoverable without loading every command's details. For an MCP wrapper, set and check a description budget during the build.

## Tier 2: reusable workflows

### 6. Keep vocabulary consistent

Prefer recognizable verbs such as `get`, `list`, `create`, `update`, and `delete`, with consistent `--json`, `--limit`, and confirmation flags. Record canonical names in command metadata and reject accidental divergence in CI.

Respect established public commands. Plan aliases or deprecation when renaming would break callers; avoid imposing a new vocabulary during an unrelated fix.

### 7. Offer three layers of introspection

Provide progressive `--help`, versioned machine-readable command metadata such as `agent-context`, and task-oriented skill instructions for composing commands. Describe flags, types, enums, defaults, mutation behavior, and conditional capabilities.

Every subcommand must support `--help` with an Examples section containing real, copyable invocations using valid flags and all required arguments. Verify those examples against the executable with disposable fixtures.

Handle help before authentication, required-argument validation, setup, network calls, or file writes. Every help invocation must succeed offline without credentials and must never trigger setup or a mutation.

Generate these from shared definitions where practical, or check them against the executable. Separate the CLI version from the introspection schema version. Expose profile names and redacted settings, never secrets.

### 8. Own asynchronous execution

For remote job submissions, provide `--wait` with bounded backoff, jitter, and a timeout. Persist recoverable job identity and expose commands such as `jobs list`, `jobs get`, and local-ledger `jobs prune`.

An interrupted wait must leave a resumable job, not trigger a second submission. A local ledger alone cannot close a crash between remote acceptance and local persistence; use backend idempotency or authoritative lookup. State explicitly whether a timeout cancels the job.

### 9. Persist reusable configuration

When workflows repeat configuration, support named profiles with save, use, list, show, and delete operations, plus a root `--profile` selector. Resolve explicit flags over environment variables, then profile values, then defaults.

Keep profile storage stable and discoverable through introspection. Separate credentials from ordinary settings and redact sensitive values in all output.

### 10. Support delivery and feedback

When artifacts need a destination, define explicit delivery schemes such as stdout, atomic file writes, and configured webhooks. Validate schemes before submitting expensive work, enumerate supported schemes on refusal, and return a structured delivery receipt.

Keep feedback local by default. Expose configured upstream delivery through introspection, with remote sending requiring user intent. Preserve artifact identity after a failed delivery so retrying delivery does not regenerate the artifact.

## Verification and handoff

Fix blockers that hang, corrupt state, duplicate mutations, or prevent parsing before adding conditional workflow features. Treat extra lookups and inconsistent flags as friction. Use shared definitions and contract checks to prevent recurrence; do not require a code generator rewrite for a small CLI.

Report the workflow covered, changed files or audit findings, checks with observed results, compatibility impact, and remaining limitations. For audit findings, use `principle | command | evidence | impact | smallest useful fix`. Distinguish verified behavior from a proposed contract.
