# Review Checklist

Use this reference before finalizing Flutter UI work or when asked for a design review.

## Product fit

- The first screen shows the actual product experience.
- The main user object is visually dominant.
- Visual density matches the user's task.
- Primary and secondary actions are distinguishable.
- Error and empty states support recovery.

## Layout

- Long labels and large numbers do not break controls.
- Null, empty, and literal "null" data renders as a deliberate placeholder or hides the optional widget.
- Text does not overlap adjacent content.
- Media, cards, grids, and toolbars have stable dimensions.
- Scroll behavior is predictable and state is preserved where expected.

## Theme

- Feature widgets do not scatter hard-coded colors and type.
- Light and dark mode are both considered.
- Status colors include non-color cues.
- Component themes or shared widgets own repeated styling.
- Shape and elevation express hierarchy without making every surface a card.

## Interaction

- Buttons, inputs, menus, tabs, nav, sheets, dialogs, and snack bars expose expected states.
- Hover, focus, keyboard, selected, loading, disabled, and pressed states are visible where relevant.
- Motion clarifies state and does not block fast users.
- Gestures have visible alternatives when discoverability matters.

## Accessibility

- Text scale has been checked for key screens.
- Tap targets meet platform expectations.
- Icon-only controls have labels/tooltips.
- Focus order is logical.
- Color contrast and semantic labels are tested or reviewed.

## Performance

- Long lists and grids are lazy.
- Build cost is localized.
- Expensive effects are justified.
- Janky animation was profiled in profile mode.
- Images are sized, cached, faded in, and given subtle loading/error states intentionally.

## Evidence

For UI changes, capture:

- Screenshot of compact width.
- Screenshot of wide layout if applicable.
- Dark mode screenshot if theme changed.
- Short video or GIF for interaction, navigation, animation, or loading.
- Test output from the repo's established checks.

## Final risk callout

Always state any unverified:

- Platform: iOS, Android, web, desktop, or other target surfaces.
- State: loading, empty, error, offline, permission, disabled.
- Mode: dark, large text, keyboard, screen reader.
- Performance: animation, scroll, image-heavy, profile-mode behavior.
