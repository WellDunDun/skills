# Text Selection

Use this reference when custom Flutter typography or brand color should also shape cursor, selection, and text-selection handles.

Source: https://flutterpro.design/details/selection-color

## Problem

MaterialApp chooses cursor and selection colors automatically. In a strongly branded app, the defaults can feel disconnected from the rest of the visual system.

## ThemeData recipe

Add textSelectionTheme to ThemeData:

    ThemeData(
      textSelectionTheme: TextSelectionThemeData(
        selectionColor: Colors.purple.withValues(alpha: 0.3),
        selectionHandleColor: Colors.purple,
        cursorColor: Colors.purple,
      ),
    )

This applies to TextField, SelectableText, and other Flutter text-selection surfaces.

## Color choice

- Cursor: use full accent or brand color.
- Handles: use full accent or brand color.
- Selection background: use the same hue with opacity so selected text stays readable.
- Light brand colors often work as-is.
- Dark brand colors usually need opacity for the selection background.

## Review checklist

- Does cursor color belong to the app's theme?
- Does selection remain readable in light and dark mode?
- Are handles visible against surrounding surfaces?
- Do editable and selectable text share the same selection language?
