# Material and Cupertino

Use this reference when deciding whether Flutter UI should be Material, Cupertino, adaptive native, or brand-led.

## Platform stance

- Material-first: Android, web, cross-platform apps, Google-style products, and products where Material components fit the brand.
- Cupertino-first: iOS-only apps or surfaces where native iOS fidelity is a product requirement.
- Adaptive native: apps that should feel at home on Android and iOS with different nav, controls, page transitions, and gestures.
- Brand-led custom: apps with a strong product language where native components are supporting pieces, not the dominant look.

## Material guidance

- Prefer current Material components and ColorScheme roles instead of legacy accent-color patterns.
- Let ThemeData.colorScheme, textTheme, and component themes drive the visual system.
- Do not over-customize Material widgets until they lose expected states, semantics, focus, and density behavior.
- Tune component themes once for repeated buttons, fields, menus, chips, sheets, cards, and snack bars.
- If the app has a custom interaction language, remove Material tap effects deliberately rather than mixing ripples with bespoke motion. See [MATERIAL_TAP_EFFECTS.md](MATERIAL_TAP_EFFECTS.md).

## Cupertino guidance

- Use Cupertino components when native iOS feel is the goal: navigation bars, segmented controls, pickers, context menus, switches, sheets, date/time input.
- Respect iOS spacing, large titles, back gestures, page transitions, and grouped settings patterns.
- Avoid mixing Material and Cupertino controls in the same small surface unless the app has a clear adaptive wrapper.
- When mixing, hide the join with a shared theme language: type, color, surface rhythm, and icon weight.

## Adaptive wrappers

- Input: date picker, switch, context menu, shortcuts, hover, focus, and drag targets.
- Dialogs/sheets: modal style, transition, width, and safe-area behavior.
- Route transitions: platform convention or custom product motion.

## Avoid

- Using Platform.isIOS as the only design system.
- Applying iOS visuals to Android while keeping Android interaction expectations broken.
- Applying Material everywhere in an iOS-only app with no product reason.
- Rebuilding common components separately for every platform when a themed shared widget is enough.

## Sources

- Material Design for Flutter: https://docs.flutter.dev/ui/design/material
- Material 3 default in Flutter: https://docs.flutter.dev/release/breaking-changes/material-3-default
- Material 3 color roles: https://docs.flutter.dev/release/breaking-changes/new-color-scheme-roles
