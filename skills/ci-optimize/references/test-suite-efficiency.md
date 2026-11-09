# Test-suite efficiency audit

Use when measured test execution accounts for substantial CI runner time. Review tests by cost and protected behavior, regardless of who wrote them.

Keep discovery time-boxed to a representative expensive job or module. Produce recommendations only. Do not delete, skip, merge, or quarantine tests, modify coverage gates, or edit project rules during this audit. Running tests may generate local artifacts; run mutation experiments in a disposable checkout and preserve the user's worktree.

## 1. Establish the baseline

- Collect per-test durations and outcomes from existing JUnit reports or test analytics. Record the runner, revision, job, retries, observation window, and available history. If reports are missing, collect a representative local run and label it as local evidence rather than historical CI data.
- Separate test execution from setup, discovery, fixtures, builds, and teardown. Summed test durations can differ from job time because of parallelism and shared work.
- Measure test count and LOC at explicit repository revisions when investigating growth. Counts of changed test files measure churn, not test count or LOC growth.
- Treat author names, bot identities, and co-author trailers as attribution clues. Allow mixed or unknown origin. Do not infer an agent's share of test runtime from its share of commits or LOC.
- Missing artifacts and renamed test IDs create gaps in failure history. Report "no observed failures in available runs," rather than claiming the test has never failed.

## 2. Find candidates cheaply

Start with expensive tests and repeated setup. Use the repo's existing lint, coverage, and search tools before adding dependencies or asking a model to review the suite.

Flag these for review, with concrete examples:

- Assertions that compare a value with itself or only exercise locally constructed fixtures without reaching the behavior under test.
- Assertion-free tests, source-text checks, export-list checks, or static-shape checks whose purpose is unclear. Successful execution and structural contracts can be valid expectations; inspect helpers and the intended contract first.
- Heavy mocking that bypasses the behavior the test claims to protect. Mock count alone does not establish low value, and call assertions can protect real collaboration contracts.
- Duplicate setup or similar bodies. Preserve input literals during semantic review: different values may exercise distinct boundaries even when normalized bodies or line coverage match.
- Frequent test changes alongside harmless internal refactors. Read representative diffs to distinguish brittle assertions from legitimate behavior changes.
- Production exports or injection seams used only by tests. Record their maintenance cost and purpose; testability alone does not make them unnecessary.

For every serious candidate, record a one-line value card answering:

1. What observable behavior does it protect?
2. What plausible regression would make it fail?
3. Does another test protect the same behavior, inputs, and failure mode?
4. Does it require production code used only by tests, and is that seam justified?

Apply two review questions: would breaking the protected behavior fail this test, and would a refactor that preserves the public contract still pass it? Use the answers to guide investigation, not as automatic deletion rules.

## 3. Validate uncertain findings

- Prefer existing regression examples or a small deliberate fault in the targeted behavior. Run the baseline first, introduce one fault, observe the expected failure, and discard the fault in the disposable checkout. Do not change assertions to manufacture a passing result.
- If mutation tooling is already configured, scope it to the costly or changed module. Record the tool, configuration, tested revision, mutation scope, and runtime budget. If setup would exceed the audit budget, propose it as follow-up rather than installing a new CI stage.
- A surviving mutant may expose a missing assertion, an equivalent mutation, or unsupported behavior. Investigate before drawing conclusions.
- A test absent from a mutant's recorded killers is not necessarily ineffective. Early bailout can credit only the first failing test, and the selected mutations may not represent the behavior it protects. Check tool settings and rerun a targeted experiment if needed.
- Line coverage, mutation score, and historical failures are complementary evidence. None alone proves a test is redundant. Unchanged or rarely exercised business rules can still need regression protection.

See [Stryker's bailout configuration](https://stryker-mutator.io/docs/stryker-js/configuration/) when interpreting per-test mutation results. [Meta's mutation-guided test generation](https://arxiv.org/abs/2501.12862) provides an example of checking generated tests against specific simulated faults; its results are not a savings forecast for this repo.

## 4. Rank recommendations

Write `test-suite-efficiency-report.md` beside the CI report:

| Test or shared fixture | Origin and confidence | Runtime per run | Executions in window | Observed failures and history gaps | Mutation evidence and scope | Protected behavior and overlap | Proposal | Estimated runner minutes saved per month and confidence |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Stable test ID or path | Known, mixed, or unknown | Measured or unavailable | Count, including retries | Available observation window | Result or not measured | Value card | Keep, optimize setup, rewrite, merge candidate, or delete candidate | Estimate and assumptions |

- Rank by expected savings and confidence. Prefer setup reuse and runtime improvements that retain assertions before recommending removal.
- Use observed execution frequency and incremental runner time. Account for worker parallelism, shared fixtures, retries, and overlapping proposals. Distinguish reduced runner consumption from reduced wall-clock time; do not price summed test durations as billed minutes without reconciliation to jobs.
- Preserve regression protection tied to known bugs. If recommending consolidation, identify exactly where equivalent behavior and boundary cases remain covered.
- Treat flaky tests separately, with a root-cause proposal and owner. Quarantine is not a cost-free substitute for repair and requires an explicit tracking and execution policy.
- For business rules, authentication, payments, and security, require responsible-owner review of any proposed reduction in protection. Keep existing required-check, coverage, and mutation gates intact.
- List unverified candidates and missing data explicitly. Produce no removal patch during discovery.

## 5. Implementation and prevention follow-up

When separately authorized, implement reviewed recommendations in small batches in the repo's CI optimization PR. Re-run relevant tests and existing gates, verify retained regression protection, and measure actual job-level savings. Rejected candidates remain unchanged.

Propose project-rule or lint changes only for demonstrated recurring problems. Prefer these criteria over test-count or mock-count quotas:

- Test behavior a user or caller relies on, including failure paths and boundaries proportional to the change's risk.
- Inspect existing coverage before adding a test. Extend it when that keeps distinct cases understandable; add a separate test when it protects a different behavior or failure mode.
- Keep expected results grounded in requirements or independently known examples. Do not mock away the behavior under test or patch production behavior inside a test to hide a failure.
- For disputed assertions, demonstrate a plausible fault they detect. Reserve mutation experiments for meaningful uncertainty rather than imposing them on every test.
- Update existing tests when the intended contract changes, explaining the change. Do not weaken assertions merely to make an implementation pass.
- During iteration, use reliable affected-test selection with a full-suite fallback. Preserve CI's required merge-candidate coverage.

If a repeated pattern admits a reliable lint rule, propose that rule with its false-positive limits. Do not add a new CI check or edit `AGENTS.md` or `CLAUDE.md` without implementation scope covering it.
