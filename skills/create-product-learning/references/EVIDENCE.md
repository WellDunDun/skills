# Evidence behind the workflow

Reviewed 2026-10-13. Use these notes when selecting an instructional approach or assessing an outcome claim. Check current documentation when configuring a service. The research supports specific learning practices and established support workflows; it does not establish a universal SaaS retention uplift.

All third-party links below are research sources, not implementation requirements. The cited products, platforms, and skills are optional examples, not dependencies to install or services to adopt. Apply relevant findings through the product's existing stack or a provider explicitly selected by the user.

## Instructional decisions

| Decision | Primary source and evidence | Application and limits |
| --- | --- | --- |
| Start with useful work and teach recovery | [Carroll et al., The Minimal Manual, IBM Research](https://research.ibm.com/publications/the-minimal-manual), 1987, reports two software-learning experiments comparing task-oriented instructions with a commercial manual | Teach necessary setup in the context of a meaningful task, show recognizable failure symptoms, and supply safe recovery. Older software context; do not transfer an effect size to modern product adoption. |
| Move from examples toward independent performance | [Atkinson, Renkl and Merrill](https://doi.org/10.1037/0022-0663.95.4.774), 2003, reports two experiments combining reduced worked steps with prompts to explain underlying principles | Demonstrate consequential choices, offer supported practice, then a task variation with fewer prompts. Adapt to prior experience. The study concerns controlled learning tasks, not SaaS customer retention. |
| Include retrieval or application with feedback | [Karpicke and Blunt](https://learninglab.psych.purdue.edu/downloads/2011/2011_Karpicke_Blunt_Science.pdf), 2011, compares retrieval practice with elaborative concept mapping in two experiments | Ask users to predict, choose, or independently perform something useful, then provide feedback. Science-text learning supports a design hypothesis for product learning, not compulsory quizzes or proof that every quiz helps. |
| Offer later practice at relevant intervals | [Cepeda et al.](https://escholarship.org/uc/item/0kp5q19x), 2008, tests fact learning and review gaps across different test delays | Choose reinforcement timing from intended reuse and user progress. Email exposure alone is not practice. Fact-recall results do not establish a universal onboarding schedule. |
| Keep the task connected to everyday use | [Merrill, First Principles of Instruction](https://mdavidmerrill.files.wordpress.com/2019/04/firstprinciplesbymerrill.pdf), 2002 | Use prior knowledge, demonstrations, attempts and integration into real work as a design framework. The paper is a theoretical synthesis and explicitly does not establish empirical support for its principles. |
| Prefer practice over assuming preferred learning styles | [Dunlosky et al.](https://doi.org/10.1177/1529100612453266), 2013, reviews ten learning techniques and their boundary conditions | Its strongest general recommendations concern practice testing and distributed practice. Choose media for the task and accessibility; the review does not prove product retention effects or optimal video length. |

## Documentation and support decisions

[Diataxis](https://diataxis.fr/start-here/) distinguishes tutorials for learning, how-to guides for an immediate task, reference for facts, and explanation for understanding. Use that distinction to avoid mixing a guided lesson with a long reference manual. Its [application guidance](https://diataxis.fr/how-to-use-diataxis/) cautions against imposing empty site structures. This is documentation practice, not an efficacy trial.

The Consortium for Service Innovation's [KCS practices](https://library.serviceinnovation.org/KCS/Knowledge-Centered_Success_Practices_Guide/402-Purpose_Behind_the_Practices) support reusing existing answers, improving them in use, capturing the requestor's context, and creating articles when a gap remains. Its [self-service guidance](https://library.serviceinnovation.org/KCS/KCS_v6/KCS_v6_Adoption_Guide/030_Build_Proficiency/010_Activities_for_Leveraging_the_KB/010_Self-Service_Success) explains that resolving known questions can change the assisted-support case mix. Apply these as support-management practices; do not import numerical targets as universal promises.

## Format selection evidence and example

Added 2026-10-18. These links are research sources, not tool requirements. [Nielsen Norman Group's onboarding guidance](https://www.nngroup.com/articles/onboarding-tutorials/) favors help in the context where users need it over upfront interface explanations. Use contextual in-app guidance as the default when safe task practice is available, then test it with the intended users. This does not establish that every interactive tour is better than video.

[Kafer, Kulesz and Wagner, 2017](https://arxiv.org/abs/1704.00074) compared equivalent software-tool text and video tutorials with 42 undergraduate students. Overall efficiency was similar in that setting; participants offered both preferred video for new material and text for lookup. This was not a comparison with contextual in-app walkthroughs and does not establish universal format superiority.

The [GMN walkthrough example](WALKTHROUGHS.md) grounds the production instructions in an actual narrated, captioned deliverable. It demonstrates a usable output format and its production boundaries. No comparative learning, support-reduction, or retention study was performed for that video.

Package research checked on 2026-10-18 identified [Driver.js](https://github.com/nilbuild/driver.js) and [React Joyride](https://github.com/gilbarbara/react-joyride) as established optional web implementation candidates. Their [interaction hooks](https://driverjs.com/docs/interactive-tours) and [React controls](https://react-joyride.com/docs/hook) support coordinating guidance with product state; they do not establish learning outcomes or replace application-owned completion checks. Use the [selection guidance](WALKTHROUGHS.md) after checking for an existing package. These links are research sources and optional implementation documentation, not required dependencies.

## Existing skills as workflow examples

Original instructions reviewed include [Corey Haines onboarding](https://github.com/coreyhaines31/marketingskills/blob/main/skills/onboarding/SKILL.md) and [emails](https://github.com/coreyhaines31/marketingskills/blob/main/skills/emails/SKILL.md), [Impeccable onboard](https://github.com/pbakaus/impeccable/blob/main/skill/reference/onboard.md), [Anthropic kb-article](https://github.com/anthropics/knowledge-work-plugins/blob/main/customer-support/skills/kb-article/SKILL.md), [PersonWise customer education](https://github.com/personwiseai/skills/blob/main/skills/personwise-customer-education/SKILL.md), and Skill Me [help documentation](https://github.com/SkillMedev/skills/blob/main/skills/help-documentation/SKILL.md) and [lifecycle mapping](https://github.com/SkillMedev/skills/blob/main/skills/lifecycle-journey-map/SKILL.md).

They inform coordination of product context, first-value tasks, existing answers, email entry/exit conditions, and maintenance. They are authoring procedures, not effectiveness studies. PersonWise is a provider-bound course workflow. Do not assume its service, credit model, or tools are available, import unsupported timing/benchmark claims, or execute third-party skills merely to inspect them.

## Business outcome claims

Skilljar's [Zenefits case study](https://www.skilljar.com/case-studies/zenefits) reports a 5% ticket reduction. Its [MasterControl case study](https://www.skilljar.com/case-studies/how-mastercontrol-scaled-self-serve-customer-education) reports fewer support calls and higher course completion. These are vendor-published customer reports; the public pages do not supply a controlled counterfactual or enough normalization detail to establish causal uplift for another product.

Skilljar's [business impact template](https://support.skilljar.com/hc/en-us/articles/38190311344781-Customer-Education-Business-Impact-Template) connects training and account outcomes. Treat trained/untrained comparisons as associations unless the study design supports causal inference. Learning retention and subscription retention are different outcomes. Measure them separately and disclose missing baseline or exposure data.
