# Product learning skill research

Research date: 2026-10-13. Target: `skills/create-product-learning`.

## Finding

Existing public skills address activation UX, lifecycle messaging, help articles, and provider-hosted courses. The useful contribution here is connecting those tasks to one evidence-backed product learning experience with shared outcomes, sources, and completion signals.

The research supports strengthening how the skill teaches and tests learning. It does not support promising higher retention merely because the agent created a help center or course. Educational experiments, support-management practices, and customer case studies answer different questions.

## Search and review method

Searched public original sources using customer education, product learning, onboarding, onboarding-cro, email-sequence, help center, knowledge base, retrieval practice, worked examples, and spacing. Skill directories served as discovery leads; the comparison below uses original instruction files. Followed renamed files and used public read-only source retrieval when browser access failed. No third-party skill was installed or executed and no third-party wording was copied.

Reviewed seven original workflows across five skill repositories, six original learning publications or author records, Diataxis, KCS, and first-party customer education reports. This is a focused investigation, not an exhaustive census of skills. License labels below were checked in the source repositories; popularity and license do not establish efficacy.

## Existing skills

| Original instructions | Scope and useful contribution | Boundary and decision |
| --- | --- | --- |
| [Corey Haines onboarding](https://github.com/coreyhaines31/marketingskills/blob/main/skills/onboarding/SKILL.md), MIT | Post-signup activation, first-run guidance, empty states, and progress | Coordinates the first useful task. It does not implement a full help and learning center. Keep the outcome mapping; omit uncited checklist-uplift numbers and fixed step counts. |
| [Corey Haines emails](https://github.com/coreyhaines31/marketingskills/blob/main/skills/emails/SKILL.md), MIT | Lifecycle strategy, copy, current-message audit, and entry/goal/exit rules | Keep behavioral sequencing and an inventory of existing messages. A suggested day schedule is a starting hypothesis, not universal learning evidence. |
| [Impeccable onboard](https://github.com/pbakaus/impeccable/blob/main/skill/reference/onboard.md), Apache-2.0 | First-run UX, contextual guidance, sample tasks, and dismiss/replay controls | Adapt guidance to prior experience and distinguish empty, failed, and denied states. Seen-state storage is not proof of successful learning or cross-device progress. |
| [PersonWise customer education](https://github.com/personwiseai/skills/blob/main/skills/personwise-customer-education/SKILL.md), MIT | Source-grounded interactive digital-human course focused on a recurring question cluster | Use targeted instruction and escalation. Its provider, OAuth, credits, and course lifecycle do not generalize to every product's infrastructure. |
| [Anthropic kb-article](https://github.com/anthropics/knowledge-work-plugins/blob/main/customer-support/skills/kb-article/SKILL.md), Apache-2.0 | Converts resolved issues into searchable, maintainable KB drafts | Add explicit reuse of canonical answers and verified resolutions. A KB-authoring procedure does not provide a learning curriculum or prove retention effects. |
| [Skill Me help documentation](https://github.com/SkillMedev/skills/blob/main/skills/help-documentation/SKILL.md), MIT | Task articles with prerequisites, actual labels, expected results, and troubleshooting | Retain task-based authoring. Verify any borrowed taxonomy against its original source rather than inherit a mistaken summary. |
| [Skill Me lifecycle mapping](https://github.com/SkillMedev/skills/blob/main/skills/lifecycle-journey-map/SKILL.md), MIT | Stage entry/exit, event inventory, current communications, and instrumentation gaps | Keep real-event mappings. Fixed dead-zone rules and excluding every calendar-based transition are too broad for products with different usage cycles. |

Verified licenses: [Corey Haines](https://github.com/coreyhaines31/marketingskills/blob/main/LICENSE), [Impeccable](https://github.com/pbakaus/impeccable/blob/main/LICENSE), [PersonWise](https://github.com/personwiseai/skills/blob/main/LICENSE), [Anthropic](https://github.com/anthropics/knowledge-work-plugins/blob/main/LICENSE), and [Skill Me](https://github.com/SkillMedev/skills/blob/main/LICENSE). Current Corey paths are `onboarding` and `emails`; older names appear in search results. None of these instruction files supplies experimental proof of customer retention improvement.

## Learning evidence and applicability

| Primary source | What was studied or proposed | How much it establishes |
| --- | --- | --- |
| [Carroll et al., The Minimal Manual, 1987](https://research.ibm.com/publications/the-minimal-manual) | Two experiments comparing task-oriented software instructions with a commercial self-instruction manual | Direct software-learning evidence, in an older office-computing context. The public author abstract supports meaningful tasks and recovery, not a modern SaaS uplift estimate. |
| [Merrill, First Principles of Instruction, 2002](https://mdavidmerrill.files.wordpress.com/2019/04/firstprinciplesbymerrill.pdf) | A synthesis of instructional theories around problems, prior knowledge, demonstration, application, and integration | A design framework. The author explicitly does not establish empirical support for the principles in this paper. |
| [Atkinson, Renkl and Merrill, 2003](https://doi.org/10.1037/0022-0663.95.4.774) | Two experiments on gradually reducing worked solution steps with self-explanation prompts | Supports testing supported-to-independent performance. Controlled learning tasks do not determine customer onboarding length or retention uplift. |
| [Karpicke and Blunt, 2011](https://learninglab.psych.purdue.edu/downloads/2011/2011_Karpicke_Blunt_Science.pdf) | Two experiments on science-text retrieval practice compared with elaborative concept mapping; later learning tests | Supports retrieval as a learning activity in the studied setting. Product application checks are a reasoned adaptation, not a finding that compulsory onboarding quizzes improve subscriptions. |
| [Cepeda et al., 2008](https://escholarship.org/uc/item/0kp5q19x) | Fact learning with varying review gaps and test delays | Spacing benefits depend on the intended retention interval. It does not establish day 1/3/7 product emails; a reminder without practice is a different intervention. |
| [Dunlosky et al., 2013](https://doi.org/10.1177/1529100612453266) | Review of ten techniques across learning contexts and boundary conditions | Rates practice testing and distributed practice highly. Educational utility ratings do not estimate commercial product retention or justify a quiz in every help article. |

The resulting learning model is an adaptation: meaningful task, adequate demonstration and support, independent application, feedback, and relevant later reuse. Test it with the product's intended users. Avoid unsupported learning-style matching, fixed media lengths, and certificates that measure only content exposure.

## Documentation and support approaches

[Diataxis](https://diataxis.fr/start-here/) separates learning tutorials from immediate how-to answers, factual reference, and explanation. Its [application guidance](https://diataxis.fr/how-to-use-diataxis/) advises working from real needs rather than imposing empty categories. The revised skill uses these distinctions without requiring four top-level folders.

[KCS practices](https://library.serviceinnovation.org/KCS/Knowledge-Centered_Success_Practices_Guide/402-Purpose_Behind_the_Practices) inform canonical-answer reuse, in-use correction, and demand-driven additions. [Self-service guidance](https://library.serviceinnovation.org/KCS/KCS_v6/KCS_v6_Adoption_Guide/030_Build_Proficiency/010_Activities_for_Leveraging_the_KB/010_Self-Service_Success) informs issue-level resolution and case-mix interpretation. These are established practice guides, not randomized demonstrations of this skill's effectiveness.

## Customer outcome evidence

| Report | Reported result | Public evidence limitation |
| --- | --- | --- |
| [Skilljar / Zenefits](https://www.skilljar.com/case-studies/zenefits) | 5% reduction in support tickets with its education initiative | Vendor-published customer case. The page does not supply controlled comparison or sufficient denominator and observation-window detail. |
| [Skilljar / MasterControl](https://www.skilljar.com/case-studies/how-mastercontrol-scaled-self-serve-customer-education) | 9% fewer support calls and 52.5% higher course completion after switching platforms | Bundled program and platform changes; insufficient public detail to isolate causes or project the numbers onto another product. |
| [Skilljar business impact template](https://support.skilljar.com/hc/en-us/articles/38190311344781-Customer-Education-Business-Impact-Template) | Connects training measures with trained/untrained account outcomes | Useful reporting structure. A template and cohort association are not a causal study. Motivation, tenure, account size, and customer-success attention can affect both training and outcomes. |

No reviewed public source establishes that this agent skill causes retention improvement. Knowledge retained by a learner and subscription retention are different outcomes.

## Changes made from the findings

| Gap in the initial skill | Revision | Location |
| --- | --- | --- |
| Task completion was specified, but learning design was thin | Behavioral objectives, pass criteria, supported practice, independent variation, and feedback | `SKILL.md`, `references/CONTENT.md` |
| Content types were mentioned without a clear selection rule | Distinguish tutorial, how-to, reference, and explanation by the user's need | `references/CONTENT.md` |
| Existing-content reuse was implicit | Canonical-answer search, verified resolution, review confidence, and support-informed corrections | `SKILL.md`, `references/CONTENT.md`, `references/VERIFY.md` |
| Emails focused on first value and suppression | Separate acquisition, recovery, and reinforcement; relevant later practice with hypothesis-based timing | `references/EMAIL.md` |
| Author walkthrough could be confused with a learning test | Independent task variation and representative-user testing with explicit unavailable-test status | `references/VERIFY.md` |
| Course engagement and business impact could be conflated | Separate exposure, competence, behavior, and results; align exposure and outcome windows; account for case mix | `references/VERIFY.md` |
| Source notes were mainly platform capabilities | Add portable evidence notes with primary learning sources, competing skills, and applicability limits | `references/EVIDENCE.md` |

## Validation scope

Format, link, and whitespace checks verify package integrity. An isolated fictional-product trial checks whether an agent can use the revised instructions to produce a coherent local content pack from supplied facts. Neither establishes real-user learning or production business outcomes. Final command results and trial limitations are reported in the chat; representative-user and live cohort evaluation remain future product-specific work.

The trial used fictional BriefBoard 0.8 facts with editor/admin/viewer permissions, an existing CSV article, a false-success defect, two actual product event schemas, and a transactional sender without orchestration. An independent agent received the revised skill, those facts, and a content-only request. The inspected output:

- Preserved the existing CSV answer and distinguished missing headers from a defect with no verified workaround.
- Used the editor's CSV path without requiring admin setup or publishing, which would notify viewers.
- Produced a worked example, guided attempt, independent changed-data task, pass criteria, hints, and feedback.
- Kept actor/workspace event identity separate from teammate activity and labeled activation as a hypothesis.
- Delivered email specifications and drafts while identifying missing orchestration, subscription data, public URLs, and send-time suppression.
- Reported runtime tests, actual learner performance, and business effects as untested.

These are observed decisions in one synthetic content-generation trial, not a controlled before/after benchmark or a real-product study. Trial inputs and outputs were kept under `/private/tmp/product-learning-trial/`, outside the repository. The revised skill passed `skills-ref validate` and the skill-creator validator; JSON, relative-link, and whitespace checks also passed.
