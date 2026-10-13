# Email learning setup

## Discover capabilities first

Use the product's existing email and scheduling stack. Keep the lesson content and sequence specification provider-agnostic. Identify the installed sender, existing campaigns, template system, subscription settings, event ingestion, scheduling, and test environment. Check current official documentation for that provider before using an API or configuring campaign behavior.

A sending API does not necessarily provide sequences. If orchestration is absent, implement it through an existing authorized scheduler or job system when the request includes implementation. Otherwise supply a portable sequence specification and describe the missing orchestration. Do not add a service or dependency solely to make the plan look complete.

## Sequence specification

Use the same outcome and lesson IDs as the learning center. For every message record:

| ID, lesson, and purpose | Eligible audience | Entry event | Delay or wait condition | Send-time guard | Subject and preview | Body and primary action | Destination | Exit/suppression | Re-entry and deduplication |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

Tie the message to the next unfinished useful task. Write concise teaching copy with one primary action and a relevant help link. Include accessible HTML and a plain-text alternative where the provider supports them. Use safe fallbacks for missing personalization and the product's existing sender identity.

Distinguish initial skill acquisition, recovery from a blocker, and later reinforcement. A reinforcement message should invite a useful new application, prediction, or independent attempt and link to feedback. Re-sending a tutorial alone is not spaced practice. Use the natural workflow cadence and observed progress to propose timing; test it rather than presenting day 1/3/7 or a fixed message count as proven. Keep completed onboarding reminders suppressed when a separate repeat-use sequence becomes relevant. See the [spacing evidence and limits](EVIDENCE.md).

Example for a hypothetical reporting product:

- A new eligible user receives a welcome lesson linking to the reporting quickstart.
- After an appropriate wait, users without a populated report receive the first-report lesson. Choose the wait from the product's use cadence and label it proposed if evidence is missing.
- Users who already created a populated report skip that lesson. Offer the next useful task only when relevant to their role and state.
- Stop first-value reminders when the activation goal is met. A separate repeat-use sequence can follow with its own eligibility and goal.

Use real event names and properties from the product. Mark proposed events as unimplemented and wire them when the task includes instrumentation. If only time-based delivery is possible, document that completed tasks may not be detectable. Do not claim behavior-based suppression works without a working state signal.

## Operational behavior

- Resolve whether eligibility and progress belong to a person, account, or workspace. Do not email every team member about an admin-only task or confuse another member's action with individual learning.
- Recheck eligibility, task completion, account state, subscription preference, and suppression at send time. Honor existing consent and unsubscribe handling; educational copy does not bypass those controls.
- Define finite waits, frequency limits, quiet hours or time zone behavior where supported, and an end to reminders. Preserve the product's established policies.
- Handle duplicate triggers, retries, and re-entry. Use stable campaign/version, recipient, and message identifiers in the existing idempotency mechanism. Do not duplicate an existing sequence.
- Verify provider exit semantics. A campaign goal may be recorded without stopping subsequent messages unless exit conditions are configured.
- Keep delivery-critical account messages separate from optional learning messages. Reuse the product's existing subscription categories.

## Configure and verify

Create or update the local integration, importable templates, or inactive provider draft within the requested scope. Record resource IDs and configuration so a later run can update them instead of creating duplicates. Never enroll real recipients or send a test message without authorization for that action. For authorized activation, verify the audience and exit behavior first.

Preview templates and test links, personalization, responsive rendering, and text alternatives. Simulate a new user, an already activated user, a user completing a task during a wait, an unsubscribed user, and a duplicate trigger. Verify already completed tasks and suppressed users do not receive queued lessons.

Report whether the sequence is a specification, local implementation, provider draft, active campaign, or observed delivery. Name missing permissions, event mappings, subscription configuration, and tests precisely.

## Research sources, not requirements

These links illustrate campaign concepts researched for this workflow. Customer.io is not required. Map those concepts to the existing stack and verify its documentation; these examples are not universal API instructions or a recommendation to change providers.

- [Customer.io exit conditions](https://docs.customer.io/messaging/send/automations/exit-conditions/) explains the distinction between completing a workflow and exiting early when goals or conditions change.
- [Customer.io campaign creation](https://docs.customer.io/journeys/create-a-campaign/) describes triggers, workflows, goals, and exit configuration.
