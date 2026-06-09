# Material Tap Effects

Use this reference when a custom Flutter design language should disable Material ripple, highlight, hover, focus, or pressed overlay effects.

Source inspiration: https://flutterpro.design/details/strip-material-tap-effects

## Why remove them

Material widgets include tap feedback by default. Ripples, highlights, and state overlays are correct for many Material-first apps, but they can clash with a custom product language that uses scale, springs, color shifts, elevation, blur, or other bespoke feedback.

Remove tap effects only when another clear interaction affordance replaces them. A silent tap target is usually worse than a Material ripple.

## Ripple vs overlay

There are two layers to disable:

- Global ink ripple/highlight: controlled by ThemeData splash, highlight, hover, and focus values.
- Material 3 component overlays: controlled by each component theme's overlayColor, splashFactory, splashRadius, or overlay shape.

Setting only ThemeData.splashFactory to NoSplash.splashFactory removes the ripple for default ink users such as InkWell, ListTile, Chip, BottomNavigationBar, NavigationRail, NavigationDrawer, and PopupMenuButton. It does not remove every Material 3 pressed, hover, or focus overlay.

## ThemeData recipe

Use one shared ButtonStyle and apply it to button-like component themes:

    final ButtonStyle noSplash = ButtonStyle(
      overlayColor: WidgetStateProperty.all(
        Colors.transparent,
      ),
      splashFactory: NoSplash.splashFactory,
    );

    ThemeData(
      splashColor: Colors.transparent,
      splashFactory: NoSplash.splashFactory,
      highlightColor: Colors.transparent,
      hoverColor: Colors.transparent,
      focusColor: Colors.transparent,

      iconButtonTheme: IconButtonThemeData(
        style: noSplash,
      ),

      floatingActionButtonTheme: FloatingActionButtonThemeData(
        splashColor: Colors.transparent,
      ),
      textButtonTheme: TextButtonThemeData(
        style: noSplash,
      ),
      elevatedButtonTheme: ElevatedButtonThemeData(
        style: noSplash,
      ),
      outlinedButtonTheme: OutlinedButtonThemeData(
        style: noSplash,
      ),
      filledButtonTheme: FilledButtonThemeData(
        style: noSplash,
      ),

      navigationBarTheme: NavigationBarThemeData(
        overlayColor: WidgetStateProperty.all(
          Colors.transparent,
        ),
      ),
      tabBarTheme: TabBarThemeData(
        overlayColor: WidgetStateProperty.all(
          Colors.transparent,
        ),
        splashFactory: NoSplash.splashFactory,
      ),

      checkboxTheme: CheckboxThemeData(
        overlayColor: WidgetStateProperty.all(
          Colors.transparent,
        ),
        splashRadius: 0,
      ),
      radioTheme: RadioThemeData(
        overlayColor: WidgetStateProperty.all(
          Colors.transparent,
        ),
        splashRadius: 0,
      ),
      switchTheme: SwitchThemeData(
        overlayColor: WidgetStateProperty.all(
          Colors.transparent,
        ),
      ),
      sliderTheme: SliderThemeData(
        overlayColor: Colors.transparent,
        overlayShape: SliderComponentShape.noOverlay,
      ),

      menuButtonTheme: MenuButtonThemeData(
        style: noSplash,
      ),
      segmentedButtonTheme: SegmentedButtonThemeData(
        style: noSplash,
      ),
      toggleButtonsTheme: ToggleButtonsThemeData(
        splashColor: Colors.transparent,
        highlightColor: Colors.transparent,
      ),
      searchBarTheme: SearchBarThemeData(
        overlayColor: WidgetStateProperty.all(
          Colors.transparent,
        ),
      ),
    )

If a component theme already exists, merge these values into the existing theme instead of replacing unrelated styling.

## Keep affordance

After removing Material effects, add or preserve another visible feedback system:

- Pressed scale or spring compression.
- Tonal color shift.
- Border or elevation change.
- Icon/text opacity change.
- Haptic feedback where platform-appropriate.
- Focus ring for keyboard users.
- Hover state for pointer users.

Do not remove focus visibility. If focus overlays are transparent, provide an explicit focus border, glow, outline, underline, or other accessible indicator.

## Review checklist

- Do all interactive Material components lose both ripple and overlay effects?
- Did existing component theme values survive the merge?
- Is hover still visible on desktop/web?
- Is keyboard focus visible without relying on the removed focus overlay?
- Are selected, pressed, disabled, and loading states still distinguishable?
- Does the replacement tap feedback match the app's motion system?
