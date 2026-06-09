---
name: flutter-design-taste
description: Shapes high-taste Flutter UI implementation using product intent, Material and Cupertino fit, theming, motion, accessibility, performance, and visual QA. Use when designing, redesigning, reviewing, or polishing Flutter and Dart interfaces, Material 3 apps, Cupertino-style flows, interaction feel, theme tokens, component states, or app-specific visual systems.
---

# Flutter Design Taste

Build Flutter interfaces that feel intentional, native where needed, brand-specific where useful, and robust across screen sizes, input modes, text scale, theme mode, and performance constraints.

## Quick start

1. Inspect the existing Flutter app before changing UI: pubspec.yaml, lib/, theme files, design tokens, shared components, and screenshots if present.
2. Identify the product surface: consumer app, productivity tool, dashboard, commerce, editor, media, onboarding, or settings-heavy utility.
3. Choose the narrowest reference files below that match the task. Do not load every reference by default.
4. Preserve existing state management, route patterns, naming, and theme conventions unless the request requires changing them.
5. Implement real Flutter widgets and states, not static mockup screens.
6. Verify with Flutter checks and runtime evidence when the UI is user-visible.

## Reference map

- Product fit and visual direction: [PRODUCT_SURFACE.md](references/PRODUCT_SURFACE.md)
- Design system foundations: [DESIGN_SYSTEM.md](references/DESIGN_SYSTEM.md)
- Material and Cupertino decisions: [MATERIAL_CUPERTINO.md](references/MATERIAL_CUPERTINO.md)
- Theme tokens and component themes: [THEME_TOKENS.md](references/THEME_TOKENS.md)
- Text selection theming: [TEXT_SELECTION.md](references/TEXT_SELECTION.md)
- Google Fonts loading polish: [GOOGLE_FONTS_LOADING.md](references/GOOGLE_FONTS_LOADING.md)
- Icon precaching: [ICON_PRECACHE.md](references/ICON_PRECACHE.md)
- Material tap effect removal: [MATERIAL_TAP_EFFECTS.md](references/MATERIAL_TAP_EFFECTS.md)
- Widget composition and component quality: [COMPONENTS.md](references/COMPONENTS.md)
- Human-readable numbers: [NUMBER_FORMATTING.md](references/NUMBER_FORMATTING.md)
- Null and empty value display: [NULL_DISPLAY.md](references/NULL_DISPLAY.md)
- Motion and interaction polish: [MOTION.md](references/MOTION.md)
- Pressable spring feedback: [PRESSABLE_SPRING.md](references/PRESSABLE_SPRING.md)
- Spring animation tuning: [SPRING_ANIMATIONS.md](references/SPRING_ANIMATIONS.md)
- Haptic feedback choices: [HAPTICS.md](references/HAPTICS.md)
- Accessibility and input: [ACCESSIBILITY.md](references/ACCESSIBILITY.md)
- Smooth image loading: [IMAGE_LOADING.md](references/IMAGE_LOADING.md)
- Swipe action discoverability: [SWIPE_ACTION_HINTS.md](references/SWIPE_ACTION_HINTS.md)
- Rendering and rebuild performance: [PERFORMANCE.md](references/PERFORMANCE.md)
- Visual QA checklist: [REVIEW_CHECKLIST.md](references/REVIEW_CHECKLIST.md)
- Code examples and review prompts: [EXAMPLES.md](references/EXAMPLES.md)

## Workflow

### 1. Discover the current UI language

Look for:

- App shell: MaterialApp, CupertinoApp, theme mode, platform stance.
- Theme layer: ThemeData, ColorScheme, TextTheme, component themes, ThemeExtension, custom token files.
- Shared UI: buttons, fields, cards, list rows, app bars, empty states, error states, modals.
- Evidence surfaces: screenshots, golden snapshots if already used, simulator scripts, CI commands.

### 2. Set a design target

Before changing visuals, name the target in plain terms:

- User: who needs to scan, decide, create, buy, edit, learn, or recover.
- Context: one-handed, field use, focus work, repeated operations, expressive browsing, or dense review.
- Density: sparse, editorial, compact, operational, immersive, or form-heavy.
- Platform stance: Material-first, Cupertino-first, adaptive native, or strongly branded custom.

### 3. Implement with Flutter-native structure

- Put design decisions in theme, tokens, and reusable widgets before scattering one-off styling.
- Prefer composable StatelessWidget and small StatefulWidget boundaries over helper methods returning widgets.
- Make all important states real: loading, empty, error, success, disabled, focused, selected, pressed, offline, and permission-denied where relevant.
- Keep runtime performance visible while polishing motion, shadows, opacity, clipping, and large lists.

### 4. Verify the experience

For user-visible UI work, run the repo's established checks and capture evidence for changed states. Use the official Flutter skills for responsive layout, routing, widget tests, widget previews, integration tests, and layout-error repair rather than duplicating those workflows here.

## Guardrails

- Do not turn every surface into rounded cards. Use cards for repeated items, contained tools, and modal surfaces, not entire page sections.
- Do not make a generic gradient-heavy app unless the product explicitly calls for it.
- Do not hard-code colors, type sizes, and radii throughout feature widgets when the app has or needs a theme layer.
- Do not rely on screenshots alone for interactive flows; verify taps, focus, keyboard, scroll, loading, and failure states.

## Final response

Report:

- Files changed.
- Design direction chosen.
- Flutter checks and runtime evidence captured.
- Any unverified states, platforms, or accessibility/performance risks.
