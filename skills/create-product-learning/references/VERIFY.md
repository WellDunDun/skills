# Verification, measurement, and upkeep

## Verify the customer journey

Use a test account or local fixture representing the intended role and plan. Record the product version, environment, checks performed, and limitations.

1. Enter the help or learning center through the intended product entry point. Find a task by navigation and search.
2. Follow the instructions from the actual starting state. Confirm labels, prerequisites, links, and expected result. Verify one likely failure and its recovery or escalation route.
3. Complete, skip, revisit, and resume lessons. Confirm the saved state belongs to the right identity and reflects the actual completion mechanism.
4. Check keyboard navigation, focus, narrow layouts, and text alternatives for instructional media.
5. Preview email destinations and simulate eligibility, task completion during delays, suppression, and duplicate triggers. Use authorized test delivery only when needed to prove sending.

For an unfamiliar core task, also observe an authorized representative user attempt a small variation without coaching. Record where they stall, misunderstand a result, need a hint, or fail to recover. Revise the smallest relevant lesson or product obstacle and retry. If representative users are unavailable, leave this test pending; an author or agent completing instructions establishes procedural accuracy, not that customers learned them.

For code changes, run the project's relevant type, build, and behavior checks. Capture reviewable UI evidence when useful. A build passing does not prove the user reached the expected result. If access prevents a walkthrough, report which instructions remain unverified.

## Check the chosen format

- For in-app walkthroughs, verify the correct role and entry state, actual action/result checks, skip/resume/replay, absent or hidden targets, error recovery, keyboard access, and narrow layouts. Guidance must not advance merely because the user clicked Next when the task requires a successful action.
- For video, follow the [render and playback checks](WALKTHROUGHS.md). Inspect the exported file, listen to narration, and check captions against both speech and the visible action. Keep recordings, screenshot sequences, and illustrations accurately labeled. Frame checks alone do not verify audio or caption timing.
- Assess independent task success, errors, recovery, and help requests for the chosen format. When comparing formats, use equivalent tasks, audience roles, and starting states. Report the comparison design and limitations; do not infer learning from views or video completion.

## Measurement plan

Choose the few measures relevant to the requested scope. Define identity level, qualifying event, denominator, cohort entry, observation window, exclusion rules, and baseline or comparison. Distinguish implemented events from proposed instrumentation.

| Measure | Definition to specify |
| --- | --- |
| Activation | Eligible new users or accounts achieving the defined useful result within a stated window |
| Time to first value | Elapsed time from the defined starting point to that result; report non-activated users separately |
| Learning progress | Starts, completions, skips, resume, and abandonment by lesson; distinguish self-report from observed task completion |
| Learning performance | Independent success against the objective, hints required, recovery, and a small task variation; separate from page views or satisfaction |
| Feature adoption | Eligible users or accounts performing the target workflow within a stated window |
| Repeated use | Users repeating the valuable action at a cadence appropriate to the product |
| Support demand | Relevant support requests per active user or account over comparable windows, broken down by topic |
| Self-service outcome | Helpful feedback or verified resolution where observable, plus failed searches and escalation; article views alone do not prove resolution |
| Retention | A defined return or repeated valuable action among the original eligible cohort at a stated interval |
| Email contribution | Task completion following eligible message exposure or clicks; separate association from causal effect |

Do not claim fewer tickets prove successful education when users may have abandoned the product or lost access to support. Track failed searches, unresolved questions, and escalation alongside ticket rate. Email opens and page views are diagnostics, not the aha moment.

Use a randomized eligible comparison when feasible. Otherwise compare matched cohorts or windows and report confounders such as launch changes, audience mix, product fixes, and seasonality. Training participants may already be more motivated; a correlation between learning and retention is not proof that learning caused retention.

Separate content exposure, learning performance, subsequent product behavior, and business results. Align education exposure with the outcome window; do not classify customers by training they completed only after the measured result. Include relevant role, tenure, account size, and customer-success attention in cohort comparisons where available.

Measure self-service at the question or issue level when possible, with resolution and easy escalation. A search or article view does not equal an avoided support ticket. Break assisted requests down by known versus new issue and difficulty: removing simple repeat questions can leave a harder case mix and increase average handling time. Interpret that alongside resolution and demand rather than concluding education failed. This follows [KCS self-service guidance](https://library.serviceinnovation.org/KCS/KCS_v6/KCS_v6_Adoption_Guide/030_Build_Proficiency/010_Activities_for_Leveraging_the_KB/010_Self-Service_Success).

When metrics are inaccessible, leave a concrete instrumentation and evaluation plan. Do not invent a lift target, claim a baseline of zero, or imply a completed retention study at launch.

## Keep content aligned with the product

Maintain a source-to-content map connecting workflows, UI labels, permissions, limits, and destinations to every affected article, lesson, guide, and email. Record an owner or proposed owner and the last verified version/date.

Review affected content when those sources change, and prioritize updates using support topics, failed searches, lesson abandonment, and user feedback. Fix broken instructions first. Retire obsolete content with redirects or replacement links where supported. Update all channel variants of the same lesson together.

Use the support loop in [content formats](CONTENT.md) for support-informed updates. Keep defects on the product-fix list. See [evidence notes](EVIDENCE.md) for the research basis.

Leave review triggers or integrate with an existing release process when requested. Do not create recurring automation merely because maintenance will be needed.

## Research sources, not requirements

These links are research examples of platform capabilities. Intercom, Userflow, and Skilljar are not required tools or recommended replacements for the product's existing stack. The sources do not provide independent proof of adoption or retention effects.

- [Intercom Product Tours](https://www.intercom.com/help/en/articles/2900885-product-tours-explained) documents multi-step guidance, targeting, and links from messages or the product.
- [Intercom Help Center collections](https://www.intercom.com/help/en/articles/56647-create-collections-in-your-help-center) documents navigable organization of help articles.
- [Userflow Resource Center](https://www.userflow.com/capabilities/resource-center) describes a persistent in-product hub with checklists and behavior-based completion.
- [Skilljar customer education](https://www.skilljar.com/customer-education) describes customer learning programs and learning paths tailored to roles and needs.
