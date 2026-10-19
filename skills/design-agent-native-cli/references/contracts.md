# Example command contracts

Read this when defining new command behavior. Adapt names to the project and document compatibility changes. The `studio` CLI below is fictional.

## Results, errors, and pagination

```sh
studio posts list --json --limit 20 --cursor next-page
```

```json
{
  "posts": [{"id": "post_42", "visibility": "private"}],
  "next_cursor": null,
  "truncated": false
}
```

The result is one JSON document on stdout. Progress belongs on stderr. For an invalid enum, exit nonzero and write a structured error to stderr before any side effects:

```json
{
  "error": {
    "code": "INVALID_ARGUMENT",
    "message": "--visibility must be public, private, or unlisted; got secret",
    "flag": "--visibility",
    "allowed": ["public", "private", "unlisted"],
    "example": "studio posts create --content hi --visibility private --json"
  }
}
```

Use the project's exit taxonomy. A new CLI might document 0 for success, 2 for invalid arguments, 3 for authentication failure, 4 for a missing resource, and 5 for a transient service failure. These values are a design choice, not a universal convention.

For a truncated result, return the continuation cursor and a narrowing hint. Distinguish reaching the end from stopping because of a configured output cap. Do not silently clip JSON or omit completion state.

## Partial-success reporting

Suppose a command creates a post and then uploads an attachment:

```sh
studio posts create --content hi --attachment cover.png --idempotency-key draft-42 --json
```

If creation succeeds and the attachment fails, return a documented nonzero exit code. Preserve the created ID in a structured outcome on stdout:

```json
{
  "status": "partial_failure",
  "post": {"id": "post_42"},
  "steps": [
    {"name": "create_post", "status": "complete", "resource_id": "post_42"},
    {"name": "upload_attachment", "status": "failed", "code": "UPLOAD_FAILED"}
  ],
  "recovery": {
    "command": "studio posts attachments add post_42 --file cover.png --idempotency-key draft-42-cover --json"
  }
}
```

Write a structured `PARTIAL_FAILURE` diagnostic to stderr. Document that nonzero exits can coexist with committed side effects. Classify each step as complete, failed, or unknown according to available evidence; a lost response is not proof that nothing happened.

The recovery command retries the attachment against the existing post. Replaying the original create with its key must preserve `post_42` and follow documented resume or replay behavior. Never create a replacement post automatically. If acceptance is unknown, provide a status lookup or reconciliation step before recommending another mutation.

## stdin and pipeline chaining

Offer an explicit stdin input mode where streamed content fits the command. Document accepted input formats and how stdin interacts with flags and files. For example, these illustrative invocations validate the same configuration:

```sh
set -euo pipefail
studio config validate --stdin --json < config.json
cat config.json | studio config validate --stdin --json | jq -e '.valid == true'
```

For chaining, return a stable machine-readable value that another command can consume. Check the producer and parser before invoking the dependent command, and quote the extracted value:

```sh
set -euo pipefail
if post_id="$(studio posts list --json --limit 1 | jq -er '.posts[0].id // empty')"; then
  studio posts get "$post_id" --json
else
  exit 1
fi
```

Check malformed input, empty stdin, a failed producer, and an empty result. Pipeline failure propagation must expose upstream failures. A failed producer or missing identifier must prevent the dependent command from running.

## Retry identity and previews

```sh
studio posts create --content hi --idempotency-key draft-42 --json
studio posts create --content hi --idempotency-key draft-42 --json
studio posts delete post_42 --dry-run --json
studio posts delete post_42 --force --no-input --json
```

The two creates return the same resource ID and perform one remote create. Define the key scope, retention period, and conflict behavior. Reusing `draft-42` with a different payload fails explicitly. A new key can intentionally create identical content.

The deletion preview returns the resolved target, expected effects, and `dry_run: true`. It sends no mutation request and does not consume an idempotency key as if work had run. Confirmation flags authorize only the specified mutation.

## Stale-write protection

Expose a resource's opaque revision when the backend supports conditional writes:

```sh
studio posts get post_42 --json
```

```json
{"id": "post_42", "version": "v7", "content": "Original text"}
```

Submit the intended update with that revision:

```sh
studio posts update post_42 --content 'Revised text' --if-version v7 --json
```

If another writer has advanced the resource to `v8`, exit nonzero and write a structured conflict to stderr:

```json
{
  "error": {
    "code": "CONFLICT",
    "resource_id": "post_42",
    "expected_version": "v7",
    "current_version": "v8",
    "message": "The resource changed. Read post_42 again and reassess the update."
  }
}
```

Include the current revision only when the backend supplies it. The authoritative store must check the revision and apply the write atomically. A CLI-side read and comparison cannot prevent a race before the write.

On conflict, preserve the other writer's changes. Reread the resource, reassess the intended change, and submit an appropriate new request with the fresh revision. Do not automatically substitute a fresh version or remove the precondition to force the old payload through. A dry-run does not reserve a revision.

Apply equivalent preconditions to deletes where supported, including a delete racing with an update or replacement. If the backend cannot enforce conditional writes, state that limitation rather than promising protection from a local preflight check.

## Side-effect-free help

These commands must succeed with no credentials, no network, and no required mutation arguments or confirmation flags:

```sh
studio auth setup --help
studio posts create --help
studio posts delete --help
```

For example, the create help includes real command syntax:

```text
Usage: studio posts create --content TEXT [flags]

Examples:
  studio posts create --content 'Hello' --json
  studio posts create --content 'Hello' --idempotency-key draft-42 --json
```

Resolve help during command parsing, before credential loading or refresh, required-argument validation, setup hooks, and mutation handlers. Package enough command metadata for offline help. Do not create configuration, credentials, caches, or logs just to print usage. Missing or invalid credentials must not obstruct help.

## Machine-readable discovery

Use top-level help to discover command groups, command help to explain invocation, and structured discovery for exact contracts. A default discovery response can summarize groups; a command selector can return full detail.

```sh
studio --help
studio video render --help
studio agent-context --command 'video render'
```

An example discovery document:

```json
{
  "schema_version": "1",
  "cli_version": "0.1.0",
  "command": "video render",
  "mutates": true,
  "async": true,
  "flags": {
    "--script": {"type": "string", "required": true},
    "--wait": {"type": "boolean", "default": false},
    "--timeout": {"type": "duration", "default": "120s"},
    "--json": {"type": "boolean", "default": false}
  },
  "available_profiles": ["weekly-recap"],
  "delivery_schemes": ["stdout", "file", "webhook"],
  "feedback": {"local": true, "upstream_configured": false}
}
```

Add input/output schemas, enums, required confirmation, idempotency semantics, and workflow-skill location as applicable. Do not expose profile secrets or credential-bearing endpoint URLs. Check help, discovery, and skill examples against the same command definitions.

## Async recovery

```sh
studio video render --script episode.txt --idempotency-key episode-42 --wait --timeout 120s --json
studio jobs list --json --limit 20
studio jobs get job_42 --json
studio jobs get job_42 --wait --timeout 120s --json
```

1. Persist the request identity and scoped idempotency key before submission.
2. Submit using backend idempotency. If unavailable, define an authoritative recovery lookup and acknowledge any remaining ambiguity.
3. Save the remote job ID and status durably. Protect concurrent ledger writes.
4. Poll with bounded exponential backoff and jitter. Honor service retry hints within the remaining time budget.
5. On timeout or interruption, preserve job identity and return the current status plus the resume command. A local wait timeout does not imply remote cancellation.
6. On completion, retain artifact identity and delivery status. A collection or delivery retry reuses the finished artifact.

If acceptance is ambiguous and neither idempotency nor lookup can resolve it, report the uncertainty and stop automatic resubmission. Do not promise exactly-once execution from a local JSONL file.

Let `jobs prune` remove eligible local history with documented retention rules. Remote cancellation or deletion needs separate, explicit semantics.

## Profile resolution

Resolve each setting independently in this order:

1. Explicit command flag.
2. Environment variable.
3. Selected profile.
4. Built-in default.

For example, an explicit `--voice calm` overrides `STUDIO_VOICE=bright` and a profile's `voice: warm`. With the flag absent, the environment wins. Do not use truthiness to discard explicit `false`, `0`, or an empty value when the schema allows them. Report effective non-secret settings or their origins when diagnosing configuration.

## Delivery and feedback

```sh
studio video render --script episode.txt --deliver file:./episode.mp4 --wait --json
studio feedback 'The documented visibility value is rejected' --json
```

For artifact-producing commands, define the default destination and what stdout contains. If stdout carries binary bytes, send receipts elsewhere or reject incompatible JSON flags before submission. Preserve clean JSON stdout when stdout carries metadata.

Write file outputs to a temporary sibling, then rename atomically. Define overwrite behavior. For webhook delivery, use explicit configuration, a timeout, bounded retries, and a delivery ID when supported. Report the destination without secrets, HTTP result, artifact ID, and delivery status. Distinguish generation success from delivery failure.

An unsupported scheme produces a structured error listing supported schemes. Validate it before creating the remote job. A delivery retry operates on an existing artifact.

Feedback records stay local unless remote sending is explicitly requested or already authorized. Endpoint presence alone is not authorization. Show whether upstream sending is configured and whether it occurred; never silently include transcripts, tokens, or unrelated files.

## Source

Adapted from the user-supplied article "10 Principles for Agent-Native CLIs," which credits [trevinsays.com](https://trevinsays.com) as its blog. The article supplies the ten principles and two-tier structure. Timeout semantics, secret redaction, and submission/delivery crash handling here are implementation guidance, not verified claims about the vendors mentioned in the article.

The per-subcommand help examples and stdin/pipeline checks also draw on [Cursor's cli-for-agents skill](https://github.com/cursor/plugins/blob/main/cli-for-agent/skills/cli-for-agents/SKILL.md).

Partial-success reporting draws on [GitHub CLI's documented attachment failures](https://cli.github.com/manual/gh_issue_create). Revision preconditions draw on [Kubernetes API update semantics](https://kubernetes.io/docs/reference/using-api/api-concepts/#updates-to-existing-resources). Side-effect-free help draws on the [Workspace CLI changelog](https://github.com/googleworkspace/cli/blob/main/CHANGELOG.md), which records a fix for setup help accidentally creating a project. These are adapted CLI contracts, not claims that the fictional commands exist in those tools.
