# Flaky required checks

First classify the failure. Distinguish a reproducible product regression, test nondeterminism, environmental failure, and insufficient evidence. A passing rerun is evidence to investigate, not proof the test is safe to remove. Preserve the first failure, seed, logs and trace; report first-attempt pass rates separately from eventual pass rates.

Investigate shared test accounts/data, ordering, parallel collisions, fixed sleeps, clock/randomness, cleanup and real external services. Prefer isolated fixtures, condition-based waits, and controlled dependencies. Keep genuine integration coverage where mocks would hide failures.

If a confirmed flaky test needs temporary quarantine, propose the smallest test-level scope with an owner, reason, repair deadline, ongoing nonblocking execution, and explicit return criteria. Quarantine implementation requires explicit authorization and equivalent reliable protection for the required behavior. Do not weaken a required merge guarantee as a CI optimization. Do not blanket-disable a suite, apply global continue-on-error, or use retries to conceal a regression. Security, authorization, data-loss, and payment checks need special care and an alternative reliable gate before relaxation.

Before making a new or repaired test required, exercise it repeatedly at a stable revision, alone and with representative parallel load/order. Include relevant environments and a bounded run budget. Also verify it fails on a known fault or negative control: a test that never fails may simply be ineffective. A clean streak reduces uncertainty; it does not prove zero flake probability. Choose sample size and acceptable risk for the repository rather than inventing a universal threshold.


Record classification, evidence, repair validation, remaining uncertainty, and any proposed quarantine in the CI optimization report.
