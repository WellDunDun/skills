# Null Display

Use this reference when API data, optional model fields, serialized null values, or empty strings can reach Flutter UI text.

Source inspiration: https://flutterpro.design/details/never-show-null

## Problem

Users should never see raw implementation values such as null, the literal string "null", or a blank gap where an expected value should be. These cases look broken and communicate nothing useful.

Common causes:

- A nullable API field is displayed with value.toString().
- A backend serializes null as the literal string "null".
- A field is present but empty.
- A nullable chain short-circuits before a fallback is applied.

## String extension

Create a small extension for UI display logic:

    extension StringX on String? {
      bool get isUsable {
        return this != null && this!.isNotEmpty && this != 'null';
      }

      String orPlaceholder([String placeholder = '-']) {
        if (!isUsable) return placeholder;

        return this!;
      }
    }

Use it for visible text:

    Text(
      user.name.orPlaceholder(),
    )

Custom placeholder:

    Text(
      user.name.orPlaceholder('N/A'),
    )

## Hide optional widgets

Sometimes the right fallback is not a placeholder. Hide optional content entirely when it has no usable value:

    if (user.profession.isUsable)
      Text(
        user.profession!,
      )

Use this for secondary metadata, optional captions, subtitle rows, profile details, and tags where a dash would add noise.

## Non-string values

When the value is not a String, convert it first:

    Text(
      order.totalPrice.toString().orPlaceholder(),
    )

If any object in the chain can be null, wrap the full expression in parentheses so the extension runs on the result of the whole chain:

    Text(
      (state.cartInfo?.totalPrice?.toString()).orPlaceholder(),
    )

Without parentheses, a null value earlier in the chain can short-circuit before orPlaceholder() runs.

## Placeholder choice

Choose placeholders by context:

- Dash: compact table cells, metrics, small metadata.
- N/A: administrative or form-like surfaces where absence needs to be explicit.
- Hidden widget: optional profile fields, subtitles, supporting labels, optional tags.
- Empty state: whole section or list has no data.
- Error state: data is missing because loading failed.

Do not use a placeholder to hide a real data integrity bug. If a required field is missing, show a recoverable error or diagnostic state where appropriate.

## Review checklist

- Does any user-facing text call toString() on nullable data directly?
- Are literal "null" strings treated as unusable?
- Are empty strings treated as unusable?
- Are optional widgets hidden instead of showing noisy placeholders?
- Are required missing values handled as real errors rather than cosmetic dashes?
- Are chained nullable expressions wrapped so the placeholder is applied?
