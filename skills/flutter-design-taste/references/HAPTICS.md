# Haptics

Use this reference when Flutter interaction feedback needs touch response for confirmation, selection, warnings, errors, dragging, snapping, or high-stakes actions.

Source inspiration:

- https://pub.dev/packages/haptic_feedback
- https://developer.apple.com/design/human-interface-guidelines/playing-haptics

## Principle

The best haptic experience is usually subtle. Users may not consciously notice it, but the product feels flatter when it is removed.

Haptics should reinforce something the UI already communicates visually or semantically. Do not use haptics as the only error, success, focus, or state signal.

## Built-in vs package

Flutter's built-in HapticFeedback covers only a few generic haptic types. The haptic_feedback package provides more specific types, including success, warning, error, light, medium, heavy, rigid, soft, and selection.

If the product needs more than Flutter's built-in set, wrap the package in a helper so feature code stays declarative and does not repeat capability checks.

## Helper pattern

Cache the capability check and expose intention-named methods:

    import 'package:haptic_feedback/haptic_feedback.dart';

    class HapticsHelper {
      HapticsHelper._();

      static bool? _canVibrate;

      static Future<void> _vibrate(HapticsType type) async {
        _canVibrate ??= await Haptics.canVibrate();

        if (_canVibrate ?? false) {
          await Haptics.vibrate(type);
        }
      }

      static Future<void> success() => _vibrate(HapticsType.success);
      static Future<void> warning() => _vibrate(HapticsType.warning);
      static Future<void> error() => _vibrate(HapticsType.error);
      static Future<void> light() => _vibrate(HapticsType.light);
      static Future<void> medium() => _vibrate(HapticsType.medium);
      static Future<void> heavy() => _vibrate(HapticsType.heavy);
      static Future<void> rigid() => _vibrate(HapticsType.rigid);
      static Future<void> soft() => _vibrate(HapticsType.soft);
      static Future<void> selection() => _vibrate(HapticsType.selection);
    }

Call it from interaction code:

    await HapticsHelper.success();

Keep haptic calls close to the user action or state transition they reinforce. Avoid firing haptics from rebuild paths.

## Common pairings

Use these as starting points, then tune by product feel:

- success: purchase completes, form submits, upload finishes, task marked done.
- warning: incomplete form, back with unsaved changes, storage almost full.
- error: wrong password, payment declined, network request failed.
- light: like button, switch toggle, bottom sheet open, tab item tap.
- medium: pull-to-refresh trigger, message long press, drag-and-drop pick up/drop, slider snap point.
- heavy: account deletion confirmation, clear all data, force close session, high-value transaction completion.
- rigid: element snaps to grid, resize handle snaps, board item rearranges, precise drag target locks.
- soft: gentle checkbox toggle, calm settings change, relaxation app confirmation.
- selection: date picker scroll, dropdown pick, option swipe, color or size selection.

## Use restraint

- Do not haptic every tap in a dense app.
- Do not use heavy haptics for routine actions.
- Do not stack multiple haptics during one gesture unless the product is explicitly tactile, such as a picker or editor.
- Avoid haptics for background state changes the user did not directly cause.
- Match haptic intensity to the emotional and practical weight of the action.

## Platform and accessibility

- Only fire haptics when the device supports vibration.
- Expect differences across iOS, Android, and hardware models.
- Respect platform norms: iOS tends to reward subtle selection/impact patterns; Android behavior varies by device and OEM.
- Do not make haptics required for understanding the UI.
- Provide visible, semantic, and audio-independent feedback for important states.

## Review checklist

- Is the haptic tied to a specific user action or important state transition?
- Does the haptic type match the action's weight?
- Would users miss it if removed, rather than be distracted by it when present?
- Is capability checked once instead of repeated throughout feature code?
- Does the UI still communicate the state when haptics are unavailable?
- Are dense repeated interactions protected from haptic spam?
