# Design System Foundations

Use this reference when creating or aligning a Flutter design system.

## Foundation order

1. Color roles: semantic surfaces, accents, status colors, overlays, dividers, disabled states.
2. Type roles: display, headline, title, body, label, numbers, captions, tabular figures if needed.
3. Spacing: a small scale for gaps, padding, page margins, list density, and panel gutters.
4. Shape: radii by purpose, not one global pill radius for everything.
5. Elevation: when surfaces stack, how shadows or tonal surfaces express depth.
6. Iconography: one icon family, stable sizes, stroke weight, filled/outlined decision.
7. Components: buttons, inputs, selection, cards, list rows, sheets, dialogs, tabs, nav, toasts.
8. Motion: duration and curve tokens for state change, entrance, route, and emphasis.

## Token principles

- Use semantic names over visual names: surfaceRaised, actionPrimary, danger, mutedText, panelGap.
- Prefer roles that survive dark mode and brand updates.
- Keep feature widgets mostly declarative: AppButton.primary, MetricTile, TaskRow, EmptyState.
- Put repeated measurements in tokens or theme extensions before copying literal values.
- Use component themes for Material widgets when styling is global.

## Flutter theme structure

- App-level theme owns color, typography, shape, component defaults, and extensions.
- Shared components own layout and state-specific rendering.
- Feature screens compose shared components and supply product-specific content.
- Avoid passing raw colors and text styles deeply through feature trees.
- If the repo already uses design tokens generated from Figma or another source, preserve that pipeline.

## Quality bar

- Dark mode is not a color inversion; tune surfaces, borders, shadows, and elevation separately.
- Disabled states must remain legible and explainable.
- Destructive actions need proximity, confirmation, undo, or delayed execution depending on risk.
- Empty and error states should occupy the same layout rhythm as loaded content when possible.
- Loading states should reserve space to prevent layout jumps.

## Flutter sources

- Flutter themes cookbook: https://docs.flutter.dev/cookbook/design/themes
- Material Design for Flutter: https://docs.flutter.dev/ui/design/material
- Flutter typography: https://docs.flutter.dev/ui/design/text/typography
