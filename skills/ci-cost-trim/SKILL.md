---
name: ci-cost-trim
description: Audit a repo's GitHub Actions and cut CI minutes and runtime without weakening checks. Use when CI is slow or expensive, especially with coding agents pushing often.
---

# CI cost trim

Shrink CI volume first, then right-size compute. Preserve required checks and their coverage for merge candidates.

## 1. Measure, read-only

- List every file in `.github/workflows`. Record triggers, branches, path filters, concurrency, matrices, runner OS, timeouts, and caching.
- Pull the last 30 days of runs for each workflow with `gh run list` and job details through `gh api`. Paginate or set limits to cover the full window; label incomplete data. Use `codiform/gh-actions-usage` or `fchimpan/gh-workflow-stats` if already available.
- Record run count, duration, failure rate, actor, and branch. Distinguish humans, bots, and agents only where evidence supports it. Sum job durations for runner minutes; report elapsed workflow time separately. Include retries and matrix jobs, and distinguish runner minutes from billed usage.
- Investigate runs whose jobs never acquire a runner. Check annotations and runner availability before attributing them to billing or budget limits. Record gaps in actual check execution.
- Run `actionlint` for correctness and `zizmor` for security. Report unavailable tools or missing access rather than claiming a clean audit.
- Read branch protection and repository rulesets. Map required status names to jobs, including merge queue events. Treat inaccessible requirements as unknown and defer changes that depend on them.

## 2. Find volume waste

Flag each issue with run counts, runner minutes, and links or run IDs:

- Frequent schedules, especially crons that have failed for days without useful follow-up.
- Bot- or agent-triggered WIP runs on draft PRs or branches such as `codex/*`, `claude/*`, and `cursor/*`, plus repeated failures from one actor. Branch names alone do not justify skipping checks.
- Duplicate work from `push` and `pull_request` on the same commits, workflows doing the same job, or overlapping scanners.
- Missing path filters that cause full builds and tests on docs-only or unrelated config-only changes. Check whether configuration affects builds before excluding it.
- Missing concurrency groups that leave superseded runs executing.
- macOS, Windows, or large matrices on every push when equivalent merge or release coverage would suffice.
- Missing dependency caching or ineffective cache keys.

If measurements show tests account for substantial runner time, use [test-suite-efficiency.md](references/test-suite-efficiency.md) for an optional, time-boxed audit before considering compute changes. Its output is recommendations; test deletion, merging, or quarantine requires a separately authorized implementation step.

## 3. Propose fixes, cheapest first

1. Add concurrency scoped by workflow and PR number or ref, with `cancel-in-progress: ${{ github.event_name == 'pull_request' }}`. Ensure unrelated branches and workflows do not cancel each other.
2. For optional WIP work, skip draft PRs and gate bot or agent branches. Include `ready_for_review` alongside existing PR activity types and use a job condition such as `${{ github.event_name != 'pull_request' || !github.event.pull_request.draft }}`. Keep final merge-candidate coverage intact.
3. Use `dorny/paths-filter` as a step or a reliable changed-file detector to skip irrelevant heavy jobs. Use `paths-ignore` only on workflows whose checks are not required. Required workflows must still trigger and produce their expected status. A stable required gate must fail on relevant job failures or unexpected skips, and pass only for successful work or an explicitly verified irrelevant change.
4. Slow down or move noisy crons. Propose an outside uptime monitor for health checks; preserve monitoring until a replacement exists.
5. Merge duplicate workflows without changing required status names. Move expensive OS jobs and full matrices to main, merge queue, or release only when the existing merge policy still receives the required coverage before merge.
6. Evaluate test selection from changed files using the repo's tools, such as `jest --changedSince=origin/main`, `--findRelatedTests`, `vitest --changed`, `nx affected`, `turbo --affected`, or pytest-testmon. Fetch the comparison base and include recently failed tests. Fall back to the full suite when history, diff data, or dependency mapping is missing or shared configuration changes. Measure time savings and missed failures against full-suite results; do not assume published percentages apply here.
7. Keep full-suite coverage at merge or nightly as a safety net. Nightly runs do not replace existing required pre-merge checks. Avoid blind retries of deterministic failures. Fix clusters of flaky tests at their shared root cause.

Estimate savings from the measured baseline. State assumptions and avoid double-counting overlapping fixes.

## 4. Only then, compute

After volume cuts, re-measure. Recommend cheaper runners only if remaining usage justifies the work:

- Consider a spare self-hosted box for private repos only, using ephemeral, isolated runners for trusted workloads. Private-repo visibility alone does not make PR code trusted.
- Consider Ubicloud or RunsOn with AWS spot. Verify current pricing and capabilities from primary sources before recommending them.
- Benchmark two viable options on representative real workflows when authorized compute and infrastructure are available. Compare runtime, cost, reliability, and maintenance; label unperformed benchmarks as pending. Vendor claims are not measurements.
- Suggest Bazel or remote execution only for large polyglot monorepos.

## Output and guardrails

Save `ci-cost-report.md` in the repo's established reports location. Include the measurement window, data gaps, workflow totals and shares, findings, ranked fixes, compute recommendation, and validation results. Link the optional test-suite report when produced.

Start with a findings table:

| Workflow | Issue | Evidence | Estimated runner minutes saved per 30 days |
| --- | --- | --- | --- |
| Workflow path | Concrete waste | Run IDs, counts, minutes | Estimate and assumptions |

Then prepare one PR per repo with the changes. Create it when publishing is within the user's authorized scope; otherwise provide the local diff and PR description. Pin added or changed third-party actions to verified full commit SHAs.

- Do not rename or remove required status checks, change protection rules to bypass them, weaken their merge-candidate coverage, or touch secrets.
- Preserve permissions and trust boundaries. Do not execute untrusted PR code in a privileged context to enable filtering or caching.
- Run `actionlint` and `zizmor` again after edits and review the diff against the required-check map.
- Verify with a docs-only test PR, where irrelevant heavy jobs skip and required statuses complete, and a code test PR, where relevant jobs run and their failures block the required gate. Cover `ready_for_review` and `merge_group` when configured. Use authorized branches and PRs; otherwise report these live checks as pending.
- Report measured savings separately from estimates, validation results, and any incomplete live verification.
- Include a follow-up measurement plan for two weeks after merging. Compare runner minutes per relevant run and total usage, accounting for changes in run volume and workload. Perform it when requested or through an authorized scheduled task; otherwise mark it pending.
