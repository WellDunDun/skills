# Accessibility and Input

Use this reference when designing or reviewing accessible Flutter UI. Use official Flutter testing skills for the mechanics of adding widget tests.

## Baseline

- Text must remain usable at large text scale and display scale.
- Interactive targets should satisfy platform target-size expectations.
- Important controls need accessible labels.
- Color cannot be the only indicator of meaning.
- Focus order should match visual and task order.
- Keyboard, mouse, trackpad, touch, and screen reader flows should all work where the platform supports them.
- Haptics must reinforce visible or semantic feedback, not replace it.

## Semantics

- Use built-in Material and Cupertino controls when possible; they include useful semantics by default.
- Add Semantics labels and values for custom controls, charts, icon-only buttons, and canvas-like UI.
- Use MergeSemantics for controls whose label and value are visually split but conceptually one item.
- Use ExcludeSemantics only when duplicate decorative content would confuse assistive tech.
- Test destructive and disabled states with screen readers when practical.

## Text scale and layout

- Avoid fixed-height containers around text unless overflow behavior is deliberate.
- Let buttons wrap or use shorter labels at compact widths.
- Keep line height readable at large scale.
- Check dialogs, sheets, nav bars, and segmented controls at large text scale.
- Avoid absolute-positioned labels that collide when text scales.

## Keyboard and focus

- Use visible focus states.
- Provide shortcuts for frequent tool or editor commands.
- Keep modal focus trapped inside active dialogs/sheets.
- Ensure Escape/back/dismiss behavior is predictable on desktop and web.
- Use FocusTraversalGroup when custom layout needs explicit traversal order.

## Sources

- Flutter accessibility overview: https://docs.flutter.dev/ui/accessibility-and-internationalization
- UI design and styling accessibility: https://docs.flutter.dev/ui/accessibility/ui-design-and-styling
- Accessibility widgets: https://docs.flutter.dev/ui/widgets/accessibility
- Adaptive user input and accessibility: https://docs.flutter.dev/ui/adaptive-responsive/input
