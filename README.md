# Daniel Petro Skills

Small, composable Agent Skills for giving coding agents better feedback loops.

These skills are meant to be practical project tools: discover how a repo builds, prove changes with the right checks, and leave future agents with a loop they can actually run.

## Quickstart

Install it with the skills installer:

```sh
npx skills@latest add WellDunDun/skills
```

Then select the skills you want to install in your coding agent.

## Why these skills exist

Agents work better when each project has a clear feedback loop. Most failures come from guessing commands, skipping runtime checks, or finishing without evidence that the changed behavior works.

The skills in this repo focus on making those loops explicit: self-driving agent prompts, build commands, targeted tests, full checks, simulator or browser verification, and reviewable evidence.

## Skills

- **[design-agent-prompt-loop](./skills/design-agent-prompt-loop/SKILL.md)** — Designs autonomous prompting loops that let coding agents drive their own next steps, handoffs, reviews, and verification prompts. Use it when you want agents to keep moving without repeated follow-up prompts.
- **[setup-build-verify-loop](./skills/setup-build-verify-loop/SKILL.md)** — Sets up project-specific build, test, runtime, and evidence verification loops for AI agents. Use it when onboarding a repo, documenting build/test commands, creating `AGENTS.md` instructions, or turning an ad hoc workflow into a repeatable loop.

## License

MIT. See [LICENSE](./LICENSE).
