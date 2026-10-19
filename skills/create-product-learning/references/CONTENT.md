# Content and learning formats

## Shared content inventory

Use the project's existing content schema. If none exists, keep a compact inventory alongside the content:

| ID | Audience and job | Outcome | Channels and paths | Product source | Completion signal | Owner | Verified version/date | Status and update trigger |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |

Link sources such as a route, relevant source lines, a verified walkthrough, release documentation, or a support theme. Keep unknowns explicit. A source file proves implementation intent; a walkthrough provides stronger evidence that the instruction works for the intended user.

## Help center

Organize by the tasks users want to accomplish. Use their search language, including real error text where appropriate. Provide a navigable home, getting-started path, search through the existing platform or an appropriate local index, and contextual entry points from the product. For content-only requests, supply the navigation and search metadata as artifacts.

Choose the content form by the user's immediate need, following [Diataxis](https://diataxis.fr/start-here/):

| Need | Form | Example for a reporting product |
| --- | --- | --- |
| Learn an unfamiliar task safely | Guided tutorial with a dependable example | Build a first report from supplied sample rows |
| Complete a known task now | Concise how-to or troubleshooting answer | Fix missing CSV headers |
| Look up an exact fact | Reference | Supported column names and types |
| Understand a consequential choice | Explanation | How report visibility affects access |

Cross-link these forms and keep long theory out of the first task. Do not create empty categories or duplicate content to satisfy a taxonomy. An experienced user should reach a direct answer without taking the beginner course.

Use a support-informed loop inspired by [KCS](https://library.serviceinnovation.org/KCS/Knowledge-Centered_Success_Practices_Guide/402-Purpose_Behind_the_Practices): search for an existing answer, verify it against the current case, improve it when necessary, and create a new answer only when a gap remains. Preserve the user's problem wording and environment, verified resolution, and confidence or review state in the inventory. Strip private case details from public content. Flag an unverified fix instead of presenting it as a solution. Review useful answers as they are reused; feed repeated issues back to product work.

Each task article should include:

- A title that names the task and a short statement of the result.
- Prerequisites, relevant role or plan restrictions, and the actual starting location.
- Ordered actions using verified UI labels, followed by the expected result.
- Common failure symptoms, safe recovery, and a support route if recovery fails.
- Related next tasks and search aliases.

Answer the question early. Use one action per step when possible. Explain consequences before an irreversible action. Never invent pricing, limits, security guarantees, or policy terms. Source these from authoritative product documentation.

Do not make a generic FAQ section to fill space. Derive questions from evidence; label anticipated questions separately if support history is unavailable. Keep a way to contact support visible when self-service fails.

## Step-through learning center

Create an actual progression through useful tasks. A typical initial path has an introduction, necessary setup, first useful result, recovery, and a next task that repeats or expands the value. Let the product's complexity determine the number of lessons.

For each lesson, define:

| Field | Requirement |
| --- | --- |
| Lesson ID and audience | Stable identity, role, and applicable product state |
| Outcome | What the user can accomplish by the end |
| Objective and pass criterion | Observable task, relevant conditions, and what counts as correct performance |
| Prerequisites | Required data, setup, permissions, and earlier tasks |
| Action | Exact steps and a working product destination |
| Practice and feedback | Safe guided attempt, a relevant independent variation, corrective feedback, and retry or hints |
| Success | Visible expected result and the completion signal |
| Recovery | Likely obstacle, relevant help, and support route |
| Next step | Next useful lesson or repeat-use task |

Example for a hypothetical reporting product: "Build your first report" ends when a populated report is available. Merely opening the report builder does not prove success. If product events are unavailable, allow "I've completed this" and label it self-reported progress. Do not silently treat it as measured activation.

For unfamiliar multi-step work, demonstrate a completed example and explain important choices. Let the learner try a partly completed task, then reduce prompts for an independent variation. Supply hints and feedback that explain the mismatch between the expected and actual result. Experienced users can go directly to independent practice or reference. A simple action may need only a short instruction.

Example objective: "Given sample rows, create a report with the expected row count and visibility." Show a worked report, guide the first import, then ask the learner to create a second report from different rows. The check compares rows and visibility with the stated criteria. It does not ask them to memorize button names. Practice must avoid live messages, spending, or destructive changes unless those actions are authorized.

Use a prediction, consequential choice, or unprompted action when a learning check helps. After the attempt, explain the correct result and offer a retry. Let help readers bypass checks. For skills worth retaining, offer another relevant application at the user's natural reuse interval. Avoid fixed lesson lengths, universal email schedules, certificates without demonstrated competence, and assumed visual/auditory learning-style matching. These choices follow the scope-limited [learning evidence](EVIDENCE.md).

For a working center, provide a path overview and accessible lesson navigation. Support back, next, skip, resume, completed lessons, and revisiting instructions. Persist progress through existing storage and identity patterns. Explain unavailable prerequisites and how to resolve them instead of trapping the learner.

Keep lesson access rules consistent with the product. Verify restricted deep links and redirects after authentication. Avoid recording customer content in learning events. Use semantic headings, keyboard access, visible focus, readable text, and captions or transcripts for instructional media.

## Contextual guidance and optional media

Choose the least intrusive format that helps the learner perform the task. Use [format selection and walkthroughs](WALKTHROUGHS.md) for contextual in-app guidance, narrated and captioned video production, and the real GMN example. Keep the same objective, terminology, help links, and task-success criteria across formats.

Use a template or sample dataset when it reduces setup effort. Clearly label sample data. Keep task instructions available as text even when video or interactive guidance is offered.
