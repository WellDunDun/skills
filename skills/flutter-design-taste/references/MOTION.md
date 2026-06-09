# Motion

Use this reference when adding or reviewing Flutter animation and interaction polish.

## Motion purposes

Use motion to:

- Show cause and effect after user action.
- Preserve object continuity across route or layout changes.
- Draw attention to a changed value or new state.
- Confirm completion, selection, expansion, or dismissal.
- Make waiting feel bounded without hiding real latency.

Do not use motion as decoration when it slows task completion or distracts from data.

## Flutter choices

- Prefer implicit animations for simple property changes: AnimatedContainer, AnimatedOpacity, AnimatedPadding, AnimatedPositioned, AnimatedSwitcher.
- Use explicit controllers when animation has choreography, gestures, scrubbing, repetition, or interruption.
- Use pressable spring feedback when replacing Material tap effects with a custom interaction language. See [PRESSABLE_SPRING.md](PRESSABLE_SPRING.md).
- Use springs for gesture release, snapping, route motion, and physical settling. See [SPRING_ANIMATIONS.md](SPRING_ANIMATIONS.md).
- Use Hero only when the same object is truly moving between routes.
- Use sliver and route animations when hierarchy changes are spatial.
- Keep animations interruptible when users can tap quickly.

## Timing

- Micro state changes: about 100-180 ms.
- Expansion, sheet, route, or panel motion: about 200-350 ms.
- Brand or onboarding moments can be longer, but avoid blocking interaction.
- Match curve and duration by purpose; do not reuse one curve everywhere.

## Interaction polish

- Add hover, focus, pressed, and selected states for desktop/web and keyboard users.
- Keep drag affordances visible where gestures are not obvious.
- Make optimistic updates reversible when network failures are plausible.
- Use haptics sparingly and only where the platform and product surface expect them. See [HAPTICS.md](HAPTICS.md).

## Reduced motion

- Respect platform animation reduction when available.
- Keep state changes understandable without motion.
- Avoid essential information conveyed only through animation.

## Performance guardrails

- Avoid animating expensive layout when transform or opacity would work.
- Avoid stacking many translucent layers.
- Profile in profile mode if animation jank appears.
- Prefer cached/static decorations over animated custom paint when the visual does not need to change every frame.

## Sources

- Flutter implicit animations: https://docs.flutter.dev/ui/animations/implicit-animations
- Simple implicit animation tutorial: https://docs.flutter.dev/learn/tutorial/implicit-animations
- Flutter rendering performance: https://docs.flutter.dev/perf/rendering-performance
