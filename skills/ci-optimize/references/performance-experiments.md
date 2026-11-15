# Performance experiments

Use after measurement identifies a bottleneck. Select the relevant experiment; do not migrate an entire toolchain to meet an arbitrary stopwatch target. Keep existing merge/release guarantees and bound experiment cost.

## Establish the comparison

- Record the hypothesis, baseline revision/configuration, candidate change, preserved checks, measurement clock, run budget, acceptance criteria, and rollback point before testing.
- Compare the same workload and intended revision. Hold runner resources, cache state, dependencies, events, and concurrency constant unless one is the variable being tested. Separate cold and warm runs; repeat measurements and report sample size and variation.
- Use a representative recent sample, such as the last 150 runs where available, within the audit's stated time window. Label truncation or extend the window explicitly. Group by workflow/job and comparable workload; do not mix unrelated jobs into one percentile.
- Measure waiting, setup, and execution p50/p95 alongside end-to-end feedback latency. Separate dependency waiting from runner provisioning where timestamps permit. Label estimates when timestamps cannot support the distinction. Job durations summed across parallel jobs measure runner consumption, not feedback latency; bucket percentiles do not add up to end-to-end p95.
- Separate successful, failed, canceled, timed-out, and incomplete runs. Track check completeness and first-attempt failures alongside latency so dropping slow runs cannot manufacture an improvement. State whether feedback means the first diagnostic or all relevant checks completing.

## Replace tools only with equivalent evidence

- Inventory the current tool's file types, rules/plugins, configuration, ignores, generated outputs, and downstream consumers. Check the candidate's current primary documentation and installed version; preserve any capability it cannot replace.
- Run old and new on the same revision with equivalent scope. Compare exit codes and diagnostics on existing violations and deliberate faults in an isolated checkout. Clean code passing both tools does not prove equivalent detection.
- List what the candidate misses, changes, or newly rejects. Retain the old tool for uncovered checks, fix the mapping, or reject the switch. Enabling every available rule does not establish parity with the old configuration.
- Evaluate faster lint/format/type-check tools only when those steps materially contribute to latency. Candidates can include Oxlint, Oxfmt, or an appropriate Bun/native TypeScript checker; verify availability and compatibility rather than assuming their commands or semantics. Check formatter output on supported languages and ensure type checking covers project references and required generated declarations.
- Package-manager/runtime migration also needs lockfile reproducibility, lifecycle scripts, workspaces, native dependencies, deployment compatibility, and an install from a clean cache. Do not delete the existing manager or runtime before those contracts are verified.
- Report runtime, missing diagnostics/capabilities, migration cost, and actual runner cost. Newer tools and vendor benchmarks do not justify a switch without repo-specific evidence.

## Caching and checkout

- Compare package-download caching, installed-dependency caching, and build caching where supported. Measure restore, validation/install, save time, and total setup; hit rate alone is not the objective.
- Seed caches from trusted main-branch runs only when provider cache scope and access rules permit PR reuse. Use keys that include relevant lockfiles, OS/architecture, runtime/tool versions, install flags, and native dependency compatibility. Never promote untrusted PR caches into privileged builds.
- If caching `node_modules`, verify the install command actually reuses it, dependencies match the lockfile, and install scripts/native outputs remain correct. Some clean-install commands discard restored dependencies. Always exercise a cache miss and stale/incompatible-cache recovery; do not assume a 95% hit rate is attainable or worth its storage/transfer cost.
- Treat clone depth and object filtering as separate choices. A blobless partial clone may avoid historical file contents while preserving useful ancestry, but checked-out content and later commands can trigger additional fetches. Benchmark total checkout plus downstream Git access, not initial fetch alone. Preserve merge-base, tag, version, and selected-file correctness using the provider notes.

## Parallel work, workers, and shards

- Run lint, type checking, builds, and tests concurrently when their inputs and resources are independent. Preserve genuine build/generated-code dependencies and failure reporting. Splitting a job adds setup/artifact overhead; measure the effect on the critical path and runner minutes.
- Separate unrelated apps or migration checks from a slow job only if their contracts, database ordering, and merge/release gates remain enforced. Review the resulting required-status map.
- Benchmark more cores versus more shards instead of always buying either first. Tune test workers to available CPU, memory, database capacity, and other concurrent processes; record total resources and monthly cost assumptions.
- Balance shards by measured duration, including setup and known slow cases. Verify the union contains every intended test, with no accidental omissions or duplicated expensive work. Give concurrent shards isolated databases, accounts, ports, or namespaces where they mutate state; cap connections and clean up after failure/cancellation.
- Disabling per-file isolation is a scoped experiment for suites whose state can be safely reset. Inventory module caches, globals, timers, mocks, environment, files, servers, and databases. Verify lifecycle cleanup behavior in the installed runner. Compare isolated and shared execution under random file order and representative parallel load; retain seeds and negative controls. Repeated passing runs reduce uncertainty without proving isolation equivalence.
- If an isolation change exposes leaks, fix the shared-state cause or keep isolation. Do not skip cases, add blind retries, or widen timeouts to make the experiment pass.

## Affected-test selection negative controls

- Compare selected tests with full-suite behavior for cross-package dependencies, shared config, generated code, dynamic loading, and missing comparison history. Keep full validation before merge wherever required; PR selection alone never proves merge safety.
- In an isolated fixture/checkout, introduce a known fault with a failing test in a downstream consumer. Verify selection includes and fails that test. Also exercise an unavailable base and an intentionally incomplete dependency mapping: selection must run full validation or fail visibly when its confidence checks cannot establish a safe subset.
- An intentionally hidden dependency can expose a selector's blind spot. Do not claim selection is sound because its selected subset passes. Repair the graph, broaden conservative invalidation, or require the full suite for that change class. Record both selector misses and the protection that catches them before merge.

## Local hooks and performance budgets

- When local feedback is a bottleneck and hook setup is in scope, add a versioned, reproducibly installed pre-push hook using existing project tooling. Measure its duration on representative diffs. Run formatting checks and lint on changed supported files; do not silently rewrite files during push.
- Check types at the affected project/dependency scope required by the checker. Passing a changed-file list can miss errors in consumers or alter project configuration semantics. Fall back to project/full checks for shared config, missing history, or uncertain selection. Handle deletions, renames, empty diffs, and filenames safely.
- Preserve CI as the authoritative gate because hooks may be absent or bypassed. Document installation and recovery; do not use a successful local hook to omit required server-side checks.
- Start performance budgets as observations. After a representative baseline, agree on repo-specific thresholds, cold-cache policy, workload/resource scope, and ownership before adding an enforcing gate. A p95 target describes a distribution, not a per-run timeout.
- Keep execution timeouts separate from performance budgets. If a budget is exceeded, report an explicit performance failure while preserving actual functional-check outcomes; cancellation or incomplete validation must block any gate that requires it. Never mark unfinished work successful or relax retries/timeouts to hide a regression.
- Implement accepted experiments in small reviewable changes. Report comparable latency percentiles, runner minutes, completeness, detected faults, and estimated/actual cost with assumptions. Reject or roll back inconclusive changes that lose required protection.
