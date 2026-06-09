# Theme Tokens

Use this reference when implementing or refactoring Flutter themes, tokens, and component defaults.

## Theme layer

Prefer this order:

1. ColorScheme for Material color roles.
2. TextTheme for typography roles.
3. Component themes for Material widgets.
4. ThemeExtension for app-specific tokens such as spacing, radii, charts, status colors, or custom surfaces.
5. Shared app widgets for repeated patterns that are not pure Material components.

## Color

- Use role-based color names in code.
- Give status colors both foreground and container/surface roles.
- Test light and dark surfaces separately.
- Avoid direct Colors.* use inside feature widgets except for throwaway prototypes or debug visuals.
- Do not use color alone for selection, errors, or status.

## Typography

- Start from Theme.of(context).textTheme.
- Use type roles by function: title, body, label, caption, data, number.
- Avoid viewport-scaled font sizes.
- Keep letter spacing at default unless matching a deliberate type system.
- Give dense operational UI smaller headings than marketing or editorial screens.
- Match cursor, selection, and handle colors to the design system. See [TEXT_SELECTION.md](TEXT_SELECTION.md).
- If using google_fonts, preload critical weights/styles before first render to avoid default-font flash. See [GOOGLE_FONTS_LOADING.md](GOOGLE_FONTS_LOADING.md).

## Spacing and shape

- Use a compact spacing scale: 4, 8, 12, 16, 20, 24, 32, 40.
- Use shape by component purpose: controls, cards, sheets, dialogs, avatars, chips.
- Avoid making every component pill-shaped.
- Keep card radius modest unless the existing brand language says otherwise.

## Component theming

Theme repeated Material widgets:

- filledButtonTheme, outlinedButtonTheme, textButtonTheme
- inputDecorationTheme
- cardTheme, dialogTheme, bottomSheetTheme
- navigationBarTheme, navigationRailTheme, tabBarTheme
- chipTheme, menuTheme, segmentedButtonTheme
- snackBarTheme, tooltipTheme

When a custom design language should not use Material ripple, highlight, hover, focus, or pressed overlays, see [MATERIAL_TAP_EFFECTS.md](MATERIAL_TAP_EFFECTS.md).

## Example token extension

    @immutable
    class AppSpacing extends ThemeExtension<AppSpacing> {
      const AppSpacing({
        required this.xs,
        required this.sm,
        required this.md,
        required this.lg,
        required this.xl,
      });

      final double xs;
      final double sm;
      final double md;
      final double lg;
      final double xl;

      @override
      AppSpacing copyWith({
        double? xs,
        double? sm,
        double? md,
        double? lg,
        double? xl,
      }) {
        return AppSpacing(
          xs: xs ?? this.xs,
          sm: sm ?? this.sm,
          md: md ?? this.md,
          lg: lg ?? this.lg,
          xl: xl ?? this.xl,
        );
      }

      @override
      AppSpacing lerp(ThemeExtension<AppSpacing>? other, double t) {
        if (other is! AppSpacing) return this;
        return AppSpacing(
          xs: lerpDouble(xs, other.xs, t)!,
          sm: lerpDouble(sm, other.sm, t)!,
          md: lerpDouble(md, other.md, t)!,
          lg: lerpDouble(lg, other.lg, t)!,
          xl: lerpDouble(xl, other.xl, t)!,
        );
      }
    }

Import dart:ui for lerpDouble.

## Sources

- Flutter themes cookbook: https://docs.flutter.dev/cookbook/design/themes
- Material 3 default in Flutter: https://docs.flutter.dev/release/breaking-changes/material-3-default
- Material 3 color roles: https://docs.flutter.dev/release/breaking-changes/new-color-scheme-roles
