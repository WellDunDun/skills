---
name: create-product-learning
description: Creates provider-agnostic product education through help centers, guided learning paths, contextual onboarding, and lifecycle email lessons grounded in actual product behavior. Use when preparing a product launch, building a customer academy or step-through learning center, helping users reach an aha moment, improving product adoption, reducing repeat support questions, or setting up an onboarding email sequence.
---

# Create product learning

Build a connected education experience that helps users achieve a useful result, recover when stuck, and return to repeat the task. Treat adoption, fewer support requests, and retention as outcomes to measure, never guaranteed effects of publishing content.

## Provider independence

- Use the product's existing application stack, content tools, components, authentication, storage, analytics, scheduler, and email integrations. Keep content and sequence definitions provider-agnostic, then map them to the capabilities actually available. Honor an explicit user choice of provider.
- Third-party names and links in this skill are research sources or optional workflow examples, not requirements. Customer.io, Intercom, Userflow, Skilljar, PersonWise, and the linked skills are not dependencies. Do not install, subscribe to, or migrate to a linked product merely because it appears in the research.
- When a capability is missing, use an appropriate implementation in the existing stack or deliver portable content and configuration with the gap labeled. Introduce a new service only within an explicit request or an approved solution to that gap.

Example request: "Use $create-product-learning to build launch education for this product, including a help center, a step-through learning center, and a draft email sequence using our existing provider."

## 1. Discover the product and delivery options

- Read project instructions, existing docs, product routes, relevant source, tests, and release notes. Walk the available product with a test account when possible. Distinguish implemented behavior from intended behavior and unverified assumptions.
- Identify audiences, roles, plans, permissions, platforms, languages, and the jobs users came to do. Inspect available support themes, search queries, and funnel data. Missing data means unknown; do not invent tickets or baseline metrics.
- Find existing help, learning, analytics, and email integrations from manifests, config names, source, and available connectors. Inspect capabilities without printing credentials. Reuse the product's stack and content conventions.
- Infer routine choices from evidence. Ask only for missing decisions that change the learning path, such as the primary audience or first useful outcome. Continue independent work while waiting.
- Honor the requested scope. For a broad launch request, create the connected experience below. For one article or sequence, create that deliverable and its necessary dependencies.

## 2. Map learning to useful outcomes

Write an adoption map before producing content. For each relevant audience, record:

| User job | First useful outcome | Prerequisites | Common obstacle and evidence | Lesson or help article | Completion signal | Next repeat-use task |
| --- | --- | --- | --- | --- | --- | --- |

Define the aha moment as an observable valuable result. Signup, viewing a tour, and checking a box are insufficient unless evidence shows they deliver the promised value. Label a proposed activation signal as a hypothesis until validated with users or data.

Prioritize the shortest path to first value and the most costly or frequent obstacles. Diagnose whether each obstacle needs teaching, simpler setup, corrected permissions, or a product fix. Cover deeper workflows after the starting path; education must not conceal defects.

Define a behavioral learning objective and a pass criterion for each core lesson. Build toward a meaningful task through a worked example, supported practice, and an independent attempt when the task warrants it. Reduce guidance for experienced users and restore hints when needed.

## 3. Create the connected experience

Read [content and learning formats](references/CONTENT.md) for the deliverables in scope.

- **Help center:** Search and improve existing answers before adding task-based navigation, a quickstart, core how-to articles, troubleshooting, and FAQs supported by real questions. Include permissions, billing, integrations, or data handling when relevant and verified. Supply search terms, related links, and a clear support route.
- **Step-through learning center:** Create ordered paths by user goal or role. Each lesson explains the result, leads to an action in the product, shows what success looks like, and offers recovery. Provide visible progress, resume, skip, and links to deeper help.
- **Contextual learning:** Add checklists, empty-state guidance, or short tours where they help a user complete the mapped task. Make guidance dismissible and available again. Use real completion signals when available.
- **Email learning:** If an email provider or sequence exists, read [email setup](references/EMAIL.md), write the lessons, and configure the supported draft or local integration within the user's authorization. If none exists, deliver portable messages and a sequence specification without choosing or buying a service.
- Add demo scripts, annotated screenshots, reusable examples, templates, or short videos where they explain a difficult action. Use verified captures or clearly labeled sample data; provide text alternatives. Avoid producing every format by default.

Reuse a shared lesson ID, outcome, terminology, source, and destination across channels. Email should send users to the same task and learning path rather than create a second curriculum.

Separate guided tutorials from quick task answers, factual reference, and deeper explanations. Link between them without forcing a four-section site. Offer useful application checks with feedback; keep ordinary help accessible without passing a quiz. Reinforce learning at relevant reuse moments through practice, not repeated exposure alone.

## 4. Implement within the requested scope

When asked to build or set up the experience, create working content, routes, navigation, progress behavior, and draft sequence configuration in the existing stack. A curriculum outline alone does not satisfy a request for a working learning center.

Use existing authoring, component, auth, storage, and analytics patterns. Preserve unrelated changes. Keep internal sources and maintenance metadata out of customer-facing prose. Scope persisted progress to the correct user or workspace; label device-only progress when that is all the implementation supports.

If product access is missing, create evidence-backed content and mark unverifiable instructions for review. If deployment or provider access is missing, deliver local implementation or importable configuration and name the remaining dependency precisely. Do not call a specification a configured service.

Prepare concrete, reviewable artifacts before requesting any additional authorization required to publish, activate, enroll recipients, or send. Proceed with actions already authorized; access to a provider alone does not authorize messaging its users.

## 5. Verify, measure, and maintain

Read [verification and measurement](references/VERIFY.md). Walk the first-value path and one recovery path using the audience's permissions. Verify links, actual UI labels, lesson actions, completion and resume behavior, accessibility, and email skip/exit behavior. Check independent performance on a small task variation; distinguish an agent walkthrough from a target-user learning test. Run checks appropriate to changed code and report unavailable runtime checks.

Record metric definitions, event mappings, identity level, baseline window, and an evaluation plan. Separate lesson completion from activation, repeated use, support demand, and retention. Keep absent analytics labeled not retrieved.

Leave a content inventory with sources, affected workflows, owner or proposed owner, verified version/date, and update triggers. When used for upkeep, update affected lessons and channels together and retire obsolete instructions.

For research rationale and its limits, read the [evidence notes](references/EVIDENCE.md). Treat existing skills as workflow examples, instructional experiments as learning evidence, and vendor case studies as reported business outcomes with methodological limits.

## Final response

Report the audience and first-value hypothesis, created files or configured resources, verified user journeys, email state, unresolved assumptions, and measurement plan. Distinguish drafted, implemented locally, tested, published, activated, and sent. Claim adoption or retention improvement only when measured.
