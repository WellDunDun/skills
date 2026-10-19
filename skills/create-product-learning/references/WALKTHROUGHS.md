# Choose and create task walkthroughs

## Choose by task and starting state

Record the audience, starting state, intended result, selected format, and reason in the adoption map or content inventory. Preserve the user's requested deliverable. The selection below guides recommendations, not a claim of measured superiority.

| Situation | Preferred starting format | Success check |
| --- | --- | --- |
| User can safely perform an unfamiliar multi-step task in the product | Contextual interactive in-app walkthrough | Actual task result, then independent repetition or variation |
| One control or prerequisite blocks an otherwise familiar task | Inline explanation, empty-state action, or concise help answer | Obstacle resolved and task continues |
| User needs the whole sequence, orientation, or a cross-role handoff explained | Optional narrated, captioned video walkthrough with a direct task link | Learner can identify the next action and then perform the relevant task |
| A live demonstration would send messages, publish, spend, or require unavailable access | Safe sample environment, verified recording/screenshots, or labeled illustrations | Explain the boundary and verify practice separately when available |
| User wants to look up an exact step or fact | Searchable how-to or reference text | Answer found and correctly applied without completing a course |

Combine formats only when they serve distinct needs. A short overview can lead into guided practice. Always let users find the direct help answer. Keep video optional and independently useful; do not force users to watch it before acting. Distinguish an interactive task walkthrough from a product tour that merely highlights features, and a screencast from an illustrated video.

## Contextual in-app walkthrough

1. Start at the relevant task or offer a clear way to launch guidance. Check role, prerequisites, existing progress, and available data. Let experienced users skip it.
2. Attach each instruction to the control or result needed now. Prefer stable component hooks to brittle selectors. Explain consequential choices in context rather than requiring a long introductory tour.
3. Advance on the meaningful action or observed result when available. Keep seen-state, self-reported learning, and verified task success separate. Handle loading, errors, missing targets, changed permissions, and already completed work.
4. Provide dismiss, resume, restart, keyboard access, and a direct help route. Reuse existing components and progress storage. Do not add a tour service merely to create overlays.
5. End with the result, a small independent practice task when useful, and the next meaningful action. Verify the walkthrough on relevant screen sizes and with the intended permissions.

## Optional walkthrough packages

Inspect manifests, lockfiles, existing walkthrough components, and their integration before choosing a package. Reuse the product's current package and installed version when they meet the task. Do not replace it solely to standardize on the candidates below; explain a concrete capability gap if a different package is needed. Honor an explicit user choice.

| When a new web walkthrough package is appropriate | Optional candidate | Documentation |
| --- | --- | --- |
| Framework-independent guidance or integration across different web frameworks | Driver.js | [Interactive tours and async/navigation hooks](https://driverjs.com/docs/interactive-tours) |
| A React application benefits from React hooks, lifecycle events, and controlled tour state | React Joyride | [Hook API](https://react-joyride.com/docs/hook) and [recipes](https://react-joyride.com/docs/recipes) |

These are optional implementation recommendations, not requirements or hosted-provider dependencies. React applications can also use Driver.js when it fits better. Existing components or concise inline help may be sufficient without either package. Do not assume these DOM-based web packages fit native mobile applications.

Verify current release, license, framework compatibility, and the installed version's API before implementation. Repository HEAD examples may differ from the published package. Keep task success, permissions, cross-route continuation, and persisted learner progress in the product's own logic. Advancing on a click is not proof that a save succeeded.

Test the full task with keyboard input, including application controls outside the tooltip. React Joyride documents [focus trapping and configuration](https://react-joyride.com/docs/accessibility); coordinate focus with the task rather than assuming library accessibility claims prove the integration works. Check asynchronous results, navigation, cancellation, and resume with the chosen package.

## Narrated, captioned video workflow

1. **Scope the teaching task.** Name the learner, start state, desired result, and why video helps. Choose only the roles and branches needed. Let the task determine length; the GMN duration is not a target for every video.
2. **Ground the storyboard.** Walk verified product steps or use supplied evidence. For each scene record the action or decision, expected result, visual source, narration, caption cue, and any unverified state. Show useful controls and outcomes rather than a sequence of feature claims.
3. **Prepare visuals and speech.** Prefer verified product capture when available. Record actual interaction where motion matters; use screenshots for stable views and clearly labeled illustrations when necessary. Remove private data, retain exact UI wording, and date/version captures. Use an existing supplied voiceover when appropriate or available authorized narration tools. Never require a particular vendor, voice service, or paid account.
4. **Synchronize and explain.** Match visual changes to the actual voiceover, not a guessed reading rate. Narrate necessary actions, choices, and results. Provide readable synchronized captions, a transcript or equivalent task text, and a caption file when the delivery channel supports one. Keep captions clear of the important controls. Explain essential visual information in speech or a descriptive text alternative. Music is optional; preserve speech clarity.
5. **Produce the requested artifact.** Use the existing media workflow or available tools. If an applicable video-production skill is installed, follow it; do not make that package a prerequisite for this portable skill. Keep editable source, storyboard/script, narration, captions, and output together under the destination project's artifact convention. A script, preview, or contact sheet is not a rendered video. If production tools or narration are unavailable, deliver the usable intermediate artifacts and name the missing step.
6. **Review the export.** Inspect the exported video at its intended display size and listen through the narration. Check exact labels, legibility, caption accuracy/timing, visible actions, pronunciation, clipped speech, transitions, private data, and the ending task link. Confirm the file plays with the intended audio and duration. Rendering success and sampled frames do not establish audio quality or task-learning effectiveness. Report checks actually performed and any missing review.

Do not perform a live publish, send, cancel, or destructive action solely to capture a lesson unless that action is authorized. Show the relevant control and explain its effect; use a safe fixture or identify a result as unverified. A screenshot sequence proves what was captured, not a completed transaction.

## Real example: GMN events walkthrough

The Global Motherhood Network team needed to learn event creation, organizers, publication, registration, and attendee access. The original `GMN-events-walkthrough.mp4` is a 95-second narrated video with captions and training illustrations. A later `GMN-events-real-screens.mp4` is 114 seconds and uses real app/site screenshots with a supplied voiceover and phrase-level captions. Both exports are 1920 by 1080 with audio. The later version is a screenshot sequence, not a continuous screen recording.

The example was checked against the owner's GMN `BRIEF.md`, `STORYBOARD.md`, `DESIGN.md`, rendered frames, and export metadata on 2026-10-18. The summary below is self-contained. GMN media, local project paths, private chats, production software, and voice services are not required or bundled. The durations describe those artifacts, not an optimal instructional length.

| Teaching beat | Concrete GMN example | How to apply the pattern |
| --- | --- | --- |
| Find the correct workspace | Events are managed in the community app's Administration > Public events, rather than the website content editor | Start with the real entry point and required role |
| Explain a consequential choice | Date-only events differ from timed events requiring start, end, and time zone | Show decision branches only when they affect the user's task |
| Separate roles and public labels | Organizer accounts grant event access; a public facilitator name alone does not | Explain permission boundaries, not only where to click |
| Teach the publication boundary | Save a draft while approval is pending; only approved public details belong on the website | Describe consequences before the live action and use safe practice |
| Connect the attendee journey | The public registration entry leads through sign-in to an in-app confirmation | Explain the handoff between roles and locations; label uncaptured results as unverified |
| Show follow-through | Notifications and View registrations lead organizers to attendees; email was not active in the captured version | End with the next task and the capabilities actually available |

For an administrator already in the app, prefer contextual guidance while they create a safe draft and review its details. Offer this video as an overview or refresher, particularly when explaining administrator/organizer/attendee handoffs. Treat attendee registration as a separate role-specific task where needed. Do not assume that watching the overview authorizes publishing a practice event or proves independent competence.

Use the GMN pattern of exact labels, visible controls, synchronized speech/captions, permission explanations, and explicit limitations. Verify the receiving product's current behavior; do not copy GMN roles, policies, or its captured email state into another product. No comparative user-learning or retention outcome is claimed for either video.
