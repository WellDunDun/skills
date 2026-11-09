# GitHub Actions: implementation checks

Read the current official documentation for the repository's action and runner versions before editing. These notes are decision checks, not a ready-to-apply workflow.

## Checkout

The official [actions/checkout README](https://github.com/actions/checkout) documents shallow checkout and history/tag controls. Inspect the version actually in the workflow. Do not bump action versions as an incidental performance change. Preserve the repository's pinning policy.

Check the selected ref for pull requests and merge groups. Fetching a branch name instead of the intended immutable SHA can test a newer or different revision. Trace scripts using `git describe`, merge bases, changed-file selection, tags, or ancestry. A performance gain is invalid if it changes what the check examines.

## Dependencies and completion events

Use `needs` for same-workflow ordering and inspect downstream conditions. Failure or skipping upstream can skip dependent jobs. A collector with `always()` must explicitly inspect upstream outcomes and reject failures, cancellation, unexpected skipping, missing results, and unknown outcomes. Intentionally skipped optional work needs a documented acceptance policy; an always-running green collector must not hide failures.

For cross-workflow work, inspect [workflow event semantics](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows). `workflow_run` completion is not proof of success, and the new workflow's default SHA may not be the triggering run's head. Validate conclusion and commit identity. Privileged follow-up workflows must not execute untrusted PR code or trust unverified artifacts. Not every webhook maps to a workflow trigger, and native event delivery has recursion and branch restrictions.

If a repository uses merge queues, check whether required workflows handle `merge_group`. PR-only success is not automatically a valid merge-group result. Keep required-check names and reporting behavior stable unless their coordinated migration is explicitly part of the task.

## Concurrency

Consult [concurrency documentation](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/control-workflow-concurrency), including pending-job replacement and any supported queue options. Do not assume a concurrency group is a durable, ordered queue.

A PR-validation group often needs both workflow identity and PR/ref identity. A shared deployment resource may need serialization across branches, but cancellation must be considered separately. A job-level noncanceling setting does not protect it from cancellation of the enclosing workflow. Avoid canceling migrations or uploads midway merely because a newer run arrived. Test behavior for pushes, pull requests, merge groups, manual runs, and tags used by this repository.

## Flaky checks

Changing branch protection is separate from changing a test file. Moving a test to another workflow can accidentally leave a required status permanently pending or silently shrink the original gate. Validate both the test selection and the resulting check statuses.

For Playwright projects, [retry documentation](https://playwright.dev/docs/test-retries) distinguishes first-run passes, flaky passes after retries, and failures. Preserve that distinction in reports. Repeated runs should use existing test tooling and isolated data, not uncontrolled access to production services.

## Review scenarios

Before accepting a change, consider: a release that needs old tags; a PR with a distant merge base; canceled or failed upstream work; a duplicate completion event; a missing artifact; an untrusted fork; a merge-group event; two concurrent deployments; and a reproducible authorization failure mislabeled flaky. Apply only the scenarios relevant to the edited workflow.
