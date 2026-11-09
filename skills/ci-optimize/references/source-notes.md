# Source and limits

`ci-optimize` combines the former `ci-cost-trim` and `ci-throughput` skills. Run-volume measurement and the test-suite audit come from the CI cost-trimming workflow; the checkout, orchestration, and flaky-check guidance below originated in the throughput package.

This skill was inspired by [@helpmeexitvi's Instagram reel](https://www.instagram.com/reel/DfckK-CguMf/), approximately 2:10, accessed November 9, 2026. The spoken content was obtained through WatchToText and cross-checked with ManyScripts. Those are speech-recognition outputs, not a manually verified verbatim transcript; incidental wording differed.

The three source ideas are selective Git history fetching, event/dependency-based orchestration instead of idle polling, and disciplined handling of flaky required tests. This skill develops them into an original operational workflow with authorization, correctness, and validation safeguards. It does not contain the transcript or the video's jokes. The video's Anthropic throughput statistic was not independently verified and is intentionally not used as evidence or a target.

Provider-specific checks are grounded in the official sources linked in [GitHub Actions notes](github-actions.md), consulted November 9, 2026. Provider behavior can change; inspect the current documentation and the versions actually installed. None of the techniques is evidence that a particular repository is slow or defective without inspecting its own workflows and runs.
