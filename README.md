# Daniel Petro Skills

Small, composable Agent Skills by [Daniel Petro](https://www.danielpetro.dev/) for product education, better agent feedback loops, sharper design judgment, and reliable verification.

Use them to help users learn a product, discover how a repo builds, prove changes with the right checks, and leave future agents with a loop they can run.

## Quickstart

Install it with the skills installer:

```sh
npx skills@latest add WellDunDun/skills
```

Then select the skills you want to install in your coding agent.

## Why these skills exist

Agents work better when each project has a clear feedback loop. Most failures come from guessing commands, skipping runtime checks, or finishing without evidence that the changed behavior works.

The skills in this repo make those loops explicit through agent prompts, project checks, and reviewable evidence. They also help agents turn verified product behavior into help articles, guided learning, and email lessons so users can reach a useful result and recover when stuck.

## Skills

- **[create-product-learning](./skills/create-product-learning/SKILL.md)** — Creates a connected help center, step-through learning paths, contextual onboarding, and email lessons from actual product behavior. Use it to prepare launch education, guide users to their first useful result, and address repeat support questions.
- **[design-agent-prompt-loop](./skills/design-agent-prompt-loop/SKILL.md)** — Designs autonomous prompting loops that let coding agents drive their own next steps, handoffs, reviews, and verification prompts. Use it when you want agents to keep moving without repeated follow-up prompts.
- **[flutter-design-taste](./skills/flutter-design-taste/SKILL.md)** — Shapes high-taste Flutter UI implementation with product fit, Material and Cupertino choices, theme tokens, motion, accessibility, and performance references.
- **[setup-build-verify-loop](./skills/setup-build-verify-loop/SKILL.md)** — Sets up project-specific build, test, runtime, and evidence verification loops for AI agents. Use it when onboarding a repo, documenting build/test commands, creating `AGENTS.md` instructions, or turning an ad hoc workflow into a repeatable loop.

## Related projects

- **[Arabic poetry](https://github.com/WellDunDun/arabic-poetry)** — A separate collaborative skill for writing, revising, explaining, and analyzing classical and modern Arabic poetry. Install it from its own repository.

## License

MIT. See [LICENSE](./LICENSE).
