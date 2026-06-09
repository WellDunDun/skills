# Components

Use this reference when implementing reusable Flutter widgets and states.

## Component boundary

Create a component when:

- A pattern appears more than once.
- The widget has meaningful states.
- The layout needs stable sizing.
- Theme usage is repeated.
- Accessibility labels or keyboard behavior must be consistent.

Do not create a component only to hide three lines of ordinary layout.

## Widget structure

- Prefer small StatelessWidget classes for reusable pieces.
- Use StatefulWidget where local interaction state belongs with the UI.
- Prefer widgets over helper methods returning widgets for reusable UI.
- Use const constructors wherever possible.
- Pass semantic domain data, not pre-styled widget fragments, unless the component is intentionally slot-based.

## State coverage

High-quality Flutter components usually include:

- Default, hover, focused, pressed, selected, disabled.
- Loading, skeleton, empty, error, success.
- Dense and comfortable sizes when the product needs density modes.
- Text overflow behavior.
- Numeric values are formatted for reading, not dumped raw. See [NUMBER_FORMATTING.md](NUMBER_FORMATTING.md).
- Null, empty, and literal "null" values never leak into user-facing text. See [NULL_DISPLAY.md](NULL_DISPLAY.md).
- Dark mode, high contrast, and text scale resilience.

## Forms and controls

- Labels should stay visible; placeholders are not labels.
- Validation should identify the problem and suggest recovery when possible.
- Destructive controls need friction proportional to risk.
- Controls should have stable width and height so progress indicators or labels do not shift layout.
- Use menus, segmented buttons, sliders, steppers, switches, and checkboxes for their expected input shape.

## Lists, rows, and cards

- List rows need primary text, secondary context, state, and action hierarchy.
- Hidden swipe actions need a one-time visual hint when discoverability matters. See [SWIPE_ACTION_HINTS.md](SWIPE_ACTION_HINTS.md).
- Repeated cards should have a clear reason to be cards: grouping, selection, comparison, or contained action.
- Avoid nesting cards inside cards.
- Use ListView.builder, SliverList, or SliverGrid for long content.
- Keep row hit targets large enough even when visual density is compact.

## Empty, loading, and error states

- Empty states should offer the next useful action.
- Loading states should reserve the loaded layout's space.
- Error states should keep retry and diagnostic details close to the failed content.
- Permission-denied states should explain what can still be done.

## Flutter component review

- Does this component read tokens from theme instead of hard-coded styling?
- Does it keep layout stable while content changes?
- Does it expose semantics and focus behavior?
- Does it rebuild only the part that changes?
- Does it fit both compact and wider parent constraints?
