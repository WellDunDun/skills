# Merge queues and compute

Read when workflow cleanup leaves substantial integration churn, queue delay, or runner consumption. Use measured workload and the existing merge policy to choose an approach. A platform switch is a proposal until implementation is authorized.

## Two-step CI and batching

- Keep useful fast checks on development pushes, then run the full required suite on the exact merge candidate before merging. Establish which mechanism enforces both stages so direct merges cannot bypass validation.
- Consider the provider's native queue or an existing integration first. Compare capabilities against actual requirements rather than introducing a queue for a repo whose PRs rarely overlap.
- Mergify is an option for two-step CI and batching. Read its current [two-step guide](https://docs.mergify.com/merge-queue/two-step/) and [queue rules](https://docs.mergify.com/merge-queue/rules/) before configuring admission versus final merge checks. Verify current plans, pricing, retention, permissions, and compatibility; do not embed a vendor discount or savings promise in the recommendation.
- Mergify can validate candidates through temporary draft PRs. Inspect the installed queue's branch prefix and event behavior; never blanket-skip all drafts or bot-authored PRs. Its default documented prefix is `mergify/merge-queue/`, but it is configurable. A prefix can select work; it does not grant privileges or prove artifact trust.
- For native GitHub queues, check `merge_group` coverage. For third-party queues, inspect their actual push/PR events. Do not assume all queues trigger the same workflow.
- Start with conservative parallelism and small batches. Batching can amortize suite setup and execution; failed batches, splitting, retries, and speculative runs can add work. Preserve attribution to the failing change and avoid rerunning known deterministic failures without a reason.
- Test an ordinary draft, ready-for-review PR, successful queue candidate, failing queue candidate, superseded run, and removed/recreated candidate where relevant. Ensure required queue work is not filtered or accidentally canceled by WIP policies.
- Compare runner minutes per merged PR, first-feedback latency, queue wait, total merge latency, failure recovery, and main-branch failures. A faster queue can consume more runner minutes; a cheaper batch can delay an individual PR. Report the tradeoff and retain rollback configuration.

## Compute only after volume cuts

- Re-measure after the inexpensive changes. Keep the current runner when remaining savings cannot justify migration and maintenance.
- Consider a spare self-hosted box for private repos only, using isolated ephemeral runners for trusted workloads. Private visibility alone does not make PR code trusted. Do not attach persistent privileged machines to untrusted PR execution.
- Ubicloud and RunsOn with AWS spot are candidates when appropriate to the repo. Verify current capabilities, pricing, interruption behavior, and operational responsibilities using primary sources before recommending them.
- Benchmark two viable options on representative real workflows when compute and infrastructure use are authorized. Compare equivalent commits, runner resources, cache states, reliability, artifact/network overhead, startup/queue delay, and total cost. Label unperformed benchmarks as pending.
- Suggest Bazel or remote execution only for large polyglot monorepos where the build graph and measured bottleneck justify the adoption cost.

Installing a GitHub app, changing merge rules, buying capacity, enabling auto-merge, or deploying runner infrastructure is not implied by a read-only optimization audit. Preserve existing guarantees and present a concrete migration and rollback plan within the authorized scope.
