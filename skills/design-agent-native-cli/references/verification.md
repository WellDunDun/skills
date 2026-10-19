# Verify observable CLI behavior

Select checks for the affected commands. Use the actual executable, a temporary configuration directory, fixture credentials, and a disposable backend or local fake service. For a design-only request, deliver these as acceptance criteria and label them unexecuted.

Do not submit real jobs, delete live resources, or send upstream feedback merely to validate a CLI contract.

## Check matrix

| Principle | Exercise | Evidence to capture |
| --- | --- | --- |
| 1. Non-interactive | Close stdin and run with pipes under a subprocess deadline. Omit a required confirmation, then supply it on a disposable target. | Prompt-free completion or a prompt-free actionable failure. A harness timeout is a failure. |
| 1, 2. stdin and chaining | Feed the same fixture through redirected stdin and a producer pipe. Chain output into a parser and a second CLI command. Repeat with malformed or empty input and an upstream failure. | Equivalent valid results, clean machine-readable output, prompt-free input failures, visible pipeline failures, and no dependent mutation after a failed producer or missing ID. |
| 2. Structured output | Parse stdout as JSON on success; parse stderr's JSON error on failure. Exercise a non-TTY run. | Valid documents, no ANSI or banners, stable identifiers, documented nonzero failure codes. |
| 2, 4. Partial success | Fail a second step after the first mutation commits, inspect state, and run the documented recovery. Also lose a response after acceptance. | Nonzero partial-failure status, committed IDs and step outcomes, honest unknown state, and recovery that preserves completed work without duplicate creates. |
| 3. Recovery errors | Pass an invalid enum, missing argument, and unknown resource. | Valid values or a discovery command, a working correction example, and no premature side effects. |
| 4. Safe retries | Repeat a create with the same key, then reuse the key with changed input. Run a deletion preview. | One remote create, the original ID on retry, an explicit conflict, and zero mutation calls during preview. |
| 4. Stale writes | Give two clients the same revision, commit one update, then submit the other's update or conditional delete. | An authoritative conflict, no write from the stale request, preserved concurrent changes, and no retry that removes or silently refreshes the precondition. |
| 5. Bounded responses | List more than the default page size and follow cursors to exhaustion. Exercise a large log or detail response. | Bounded output, accurate truncation/continuation, no missing or repeated IDs for a stable fixture dataset. |
| 6. Vocabulary | Enumerate registered command paths and flags. Compare them with the naming policy. | A schema or build check catches an inconsistent new command while allowing documented compatibility aliases. |
| 7. Introspection | Enumerate every registered subcommand, invoke its `--help`, and run its Examples invocations with disposable fixtures. Compare help and discovery metadata with parsed command definitions. | Every subcommand has working help and real examples with valid flags and required arguments. Types, defaults, enums, availability, and schema version match the executable. |
| 7. Help without side effects | Run root, group, and every subcommand's help offline with absent or invalid credentials, omitted required arguments, and no mutation confirmation. | Exit 0, useful help and examples, no prompts, no network attempts, and no configuration, credential, cache, log, or remote-state changes. |
| 8. Async execution | Interrupt a wait, restart the process, and resume by job ID or the same key. Simulate acceptance before a lost response. | One submission, recoverable identity, bounded polling, timeout semantics, and successful collection. |
| 9. Profiles | Set conflicting flag, environment, profile, and default values. Repeat in a fresh process. | Per-setting precedence, durable selection, preservation of valid false/zero values, redacted discovery. |
| 10. Delivery/feedback | Reject an invalid scheme; interrupt a file write; fail a local webhook; record feedback without upstream consent. | Early refusal, no partial final file, reusable artifact after delivery failure, and zero unintended network sends. |

## Portable non-TTY probe

Replace the command with a real read-only invocation. This example uses Python's subprocess deadline to avoid depending on a platform-specific `timeout` executable:

```python
import json
import subprocess

completed = subprocess.run(
    ["studio", "posts", "list", "--json", "--limit", "2"],
    stdin=subprocess.DEVNULL,
    capture_output=True,
    text=True,
    timeout=5,
    check=False,
)
assert completed.returncode == 0, completed.stderr
assert "\x1b" not in completed.stdout
assert "\x1b" not in completed.stderr
result = json.loads(completed.stdout)
assert len(result["posts"]) <= 2
assert "next_cursor" in result
```

For an invalid enum, assert a nonzero code, an empty result stream, the valid set in the structured error, and zero recorded backend mutation calls. A JSON parser succeeding alone does not prove the operation was correct.

## stdin and pipeline checks

Use the patterns in [contracts.md](contracts.md) with commands discovered from the actual CLI.

1. Feed a valid fixture through both file redirection and a producer pipe. Compare the parsed result with the equivalent flags or file-input invocation where supported.
2. Test malformed input and empty stdin. Require an actionable, prompt-free response within a subprocess deadline. Document whether empty input is valid.
3. Pipe machine-readable output into a parser, then pass a resulting value into a second CLI command. Check the resulting resource or state, including values containing spaces or shell metacharacters. Quote substituted values.
4. Repeat with a failed producer and a successful response containing no matching ID. Use shell pipeline failure propagation and explicit success checks so dependent commands do not run with missing or invalid values.

Capture each process's exit status, stderr, and resulting state. A final parser exiting successfully does not prove the producer succeeded. Use disposable targets for any mutation and verify that failed chains perform no dependent mutation.

## Partial-success checks

Use the post-and-attachment example in [contracts.md](contracts.md) with a fake service that records committed writes and resource IDs.

1. Fail before the first write. Confirm the outcome reports no committed steps and the backend remains unchanged.
2. Let the post commit, then fail its attachment. Require a nonzero exit, parseable outcome and diagnostic, `post_42`, the completed and failed steps, and a recovery command.
3. Execute recovery in a fresh process. Verify one post exists, its ID is unchanged, and only the failed attachment step runs. Replay the original idempotency key and verify it also creates no duplicate post.
4. Accept a write but drop its response. Require an unknown outcome unless authoritative lookup resolves it. Verify recovery inspects state before submitting any potentially duplicate work.

Compare reported outcomes with recorded backend state. A nonzero process exit does not prove rollback, and a created ID alone does not prove all requested steps succeeded.

## Stale-write checks

1. Read the same fixture revision into two independent clients. Commit client A's update and record the new state and revision.
2. Submit client B's different update using its original revision. Require a nonzero conflict and verify client A's state remains intact. The stale request must commit no write.
3. Exercise an interleaving that changes the backend after any CLI preflight read but before the conditional write. Confirm the backend still rejects the stale request.
4. Repeat with a conditional delete where supported. Verify a stale delete preserves the updated or replaced resource.
5. Reread and reassess the change, then explicitly submit a fresh conditional request. Verify it succeeds with the current revision. Missing required preconditions must fail rather than silently allow an unconditional write.

Inspect backend requests to confirm retries preserve the original precondition until the caller deliberately submits a reassessed request. If the backend lacks atomic conditional writes, record that limitation instead of marking the check passed.

## Side-effect-free help checks

Enumerate command paths from the registered definitions, including auth/setup commands and destructive commands. Run root, group, and leaf `--help` in fresh subprocesses with closed stdin and a deadline.

- Use an empty temporary configuration directory and unset credentials. Repeat with invalid or expired credentials to catch eager authentication and refresh hooks.
- Block outbound networking and record attempted connections. Help must still exit 0 without attempting authentication, discovery fetches, or setup calls.
- Omit normal required arguments and destructive confirmation flags. Help must print usage and real examples without entering a prompt or a handler.
- Compare configuration and state directories before and after, and inspect fake-service call logs. Require no credential, config, cache, log, or remote-resource writes.

Instrument network and write attempts as well as final state. A blocked request or a file created and then removed is still a side effect. Run the ordinary command separately with disposable fixtures to confirm help handling has not disabled its normal validation.

## Failure injection for remote jobs

Use a fake service or the project's existing integration harness to simulate:

- Remote acceptance followed by a lost response or process crash before ledger persistence.
- Process termination during polling, followed by recovery from a fresh process.
- Rate limiting, a transient service error, and a job that outlasts the wait deadline.
- Remote success followed by local artifact collection or webhook failure.
- Concurrent requests using the same idempotency key.

Count backend submission calls and resulting jobs, then inspect persisted state and returned IDs. Verify that timeout preserves recoverability and that delivery retries reuse the original artifact. A stored job record alone does not prove that duplicate submission is impossible.

## Report findings

For each applicable principle, distinguish verified behavior, observed failure, and untested behavior. Mark a conditional capability not applicable only with a concrete reason.

Prioritize failures by impact:

- Blocker: hangs in headless use, corrupts state, duplicates work, leaks secrets, or makes required results unparseable.
- Friction: extra discovery calls, inconsistent flags, missing narrowing hints, or manual polling and configuration that obstruct the intended workflow.
- Improvement: a relevant shared definition or workflow feature reduces repeated work without expanding the requested scope.

Use `principle | command | evidence | impact | smallest useful fix` for audit findings. Include the command, observed output or state, and the verification limitation. Do not mark a principle passed based only on documentation or a helper function's unit test.
