# AGENTS.md

## Project purpose

This repository is a public Agent Skills collection. Keep the root README user-facing: what the skills do, why they exist, how to install them, the skill list, and the license. Put maintainer-only process, validation, layout, and serving details in this file.

Do not add app prototypes, generated artifacts, screenshots, or unrelated project files to the repo root.

## Public README guidance

Keep `README.md` similar in shape to a public skills catalog:

- Short repo purpose.
- Quickstart for users.
- Why the skills exist.
- List of available skills with links and plain-language descriptions.
- License link.

Do not put detailed authoring rules, manifest internals, validation commands, or package maintenance instructions in `README.md`.

## Skill layout

Use this layout for every skill:

```text
skills/<skill-name>/
  SKILL.md
  references/        # optional, one level deep
  scripts/           # optional
  assets/            # optional
```

Rules:

- `SKILL.md` frontmatter must include `name` and `description`.
- `name` must match the parent directory exactly.
- Use lowercase letters, digits, and hyphens only for skill names.
- Keep `SKILL.md` under 100 lines when practical; move optional detail to `references/`.
- Reference files from `SKILL.md` with relative links such as `references/REFERENCE.md`.
- Avoid category folders unless the repository intentionally changes its serving layout.

## Skill authoring rules

- Prefer concise procedural instructions over broad best-practice prose.
- Ground new skills in real workflows, traces, docs, commands, or examples.
- Include concrete examples when they help the agent produce the right output shape.
- Add scripts only for deterministic repeated operations or validation logic.
- Keep reference files one level deep from `SKILL.md`.
- Keep descriptions specific and trigger-friendly: first sentence says what the skill does; second sentence starts with `Use when` and lists concrete triggers.

## Installing and serving

The served skills are listed in `.claude-plugin/plugin.json`:

```json
{
  "name": "danielpetro-skills",
  "skills": [
    "./skills/setup-build-verify-loop"
  ]
}
```

When adding, renaming, moving, or removing a served skill:

- Update `.claude-plugin/plugin.json`.
- Update the skill list in `README.md`.
- Update `skills/README.md` if it is kept as a compact local index.
- Confirm the README quickstart uses the final published GitHub slug before release.

## Validation

After changing a skill, run the Agent Skills reference validator for each changed skill:

```sh
skills-ref validate skills/<skill-name>
```

Also validate `.claude-plugin/plugin.json` after manifest edits:

```sh
python3 -m json.tool .claude-plugin/plugin.json >/dev/null
```

For this repo's current skill, the expected checks are:

```sh
skills-ref validate skills/setup-build-verify-loop
python3 -m json.tool .claude-plugin/plugin.json >/dev/null
```

Final responses for repo edits should report changed files and validation results.
