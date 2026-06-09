# Pressable Springs

Use this reference when buttons, cards, list rows, or custom controls should react physically to press instead of relying on Material ripple alone.

Sources:

- https://flutterpro.design/details/pressable-spring
- https://pub.dev/packages/motor

## When to use

Use pressable spring feedback when:

- The app has a custom interaction language.
- Material tap effects have been removed.
- A control should feel tactile, premium, or object-like.
- The pressed state should communicate with scale rather than ink.

Do not scale dense rows so much that text blurs, layout feels unstable, or repeated taps become noisy.

## Basic pattern

Use motor's SingleMotionBuilder to animate scale toward a target spring value:

    SingleMotionBuilder(
      motion: CupertinoMotion.smooth(),
      value: _scale,
      builder: (context, scale, child) {
        return Transform.scale(
          scale: scale,
          child: child,
        );
      },
      child: widget.child,
    )

## Reusable Pressable

    class Pressable extends StatefulWidget {
      const Pressable({
        super.key,
        this.onTap,
        required this.child,
      });

      final VoidCallback? onTap;
      final Widget child;

      @override
      State<Pressable> createState() => _PressableState();
    }

    class _PressableState extends State<Pressable> {
      double _scale = 1.0;

      @override
      Widget build(BuildContext context) {
        return GestureDetector(
          onTap: widget.onTap,
          onTapDown: (_) {
            setState(() {
              _scale = 0.96;
            });
          },
          onTapUp: (_) {
            setState(() {
              _scale = 1.0;
            });
          },
          onTapCancel: () {
            setState(() {
              _scale = 1.0;
            });
          },
          child: SingleMotionBuilder(
            motion: CupertinoMotion.smooth(),
            value: _scale,
            builder: (context, scale, child) {
              return Transform.scale(
                scale: scale,
                child: child,
              );
            },
            child: widget.child,
          ),
        );
      }
    }

## Platform feel

Using one high-quality spring across platforms is acceptable. If platform fit matters, use CupertinoMotion on iOS/macOS and MaterialSpringMotion on other Material-first targets.

## Review checklist

- Does the press feedback replace or complement the app's tap effect intentionally?
- Is the scale subtle enough to avoid layout instability?
- Does tap cancel return to rest?
- Is disabled state excluded from press feedback?
- Does keyboard/focus activation still show feedback?
