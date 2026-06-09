# Spring Animations

Use this reference when Flutter motion should feel physical: gesture release, drag-to-dismiss, swipe cards, snapping pages, picker wheels, scroll bounce-back, and route transitions.

Sources:

- https://flutterpro.design/details/spring-physics
- https://pub.dev/packages/motor

## Mental model

A spring moves like a real object returning to rest. It does not travel at a fixed speed, stop instantly, or follow a purely duration-based easing curve. It can overshoot, wobble, and settle.

Flutter describes a spring with three values:

    SpringDescription(
      mass: 1.0,
      stiffness: 100.0,
      damping: 10.0,
    )

## Tuning values

Mass controls how heavy the animated thing feels:

- Low mass, around 0.3: quick, light, snappy.
- Default-ish mass, around 1.0: neutral.
- High mass, around 2.0: slower to start, slower to stop, heavier.

Stiffness controls how strongly the spring pulls back:

- Low stiffness, around 50: soft, slower, takes longer to settle.
- Medium stiffness, around 100-250: useful for many interface motions.
- High stiffness, around 500: sharp, fast, snaps into place quickly.

Damping controls how quickly bouncing stops:

- Low damping, around 3: bounces many times.
- Medium damping, around 10-20: lively but controlled.
- High damping, around 30: barely bounces, settles quickly.
- Zero damping: bounces forever; avoid it for product UI.

There are no universal correct values. Tune until the surface feels right for the object, platform, density, and task.

## AnimationController springs

Drive a spring directly with AnimationController.animateWith and SpringSimulation:

    _controller.animateWith(
      SpringSimulation(
        const SpringDescription(
          mass: 0.5,
          stiffness: 200,
          damping: 18,
        ),
        _controller.value,
        1.0,
        0,
      ),
    );

Use this where forward() or animateTo() would otherwise produce mechanical motion.

For gesture release, pass release velocity into the simulation so the spring continues the user's motion:

    _controller.animateWith(
      SpringSimulation(
        const SpringDescription(
          mass: 0.5,
          stiffness: 200,
          damping: 18,
        ),
        _controller.value,
        1.0,
        releaseVelocity,
      ),
    );

Important: use AnimationController.unbounded(vsync: this) when overshoot matters. A normal AnimationController clamps values to 0-1, which can hide the bounce.

## Scroll physics springs

Scrollable widgets accept a physics parameter. Override the spring getter inside custom physics when snapping or bounce-back should feel tuned:

    class SnappyPagePhysics extends PageScrollPhysics {
      const SnappyPagePhysics({super.parent});

      @override
      SpringDescription get spring {
        return const SpringDescription(
          mass: 0.5,
          stiffness: 200,
          damping: 18,
        );
      }

      @override
      SnappyPagePhysics applyTo(ScrollPhysics? ancestor) {
        return SnappyPagePhysics(
          parent: buildParent(ancestor),
        );
      }
    }

Use it like:

    PageView(
      physics: const SnappyPagePhysics(),
      children: pages,
    )

Useful surfaces:

- PageView and TabBarView for snapping between pages.
- ListWheelScrollView for picker-like snapping.
- CarouselView for item snapping.
- ListView, GridView, SingleChildScrollView, and CustomScrollView for edge bounce-back.
- NestedScrollView and ReorderableListView when their scroll feel matters.

For snapping widgets, users feel the spring on every swipe. For free-scrolling lists, the spring mainly drives edge bounce-back, and only under BouncingScrollPhysics. Android's ClampingScrollPhysics uses a glow rather than a bounce.

## Route springs

Route transitions can use springs too. Override TransitionRoute.createSimulation() to return a SpringSimulation instead of relying only on a duration and curve.

Use spring route motion when:

- A sheet, panel, or page should feel physically attached to the gesture.
- Exit velocity should affect dismissal.
- The transition represents a spatial object moving into place.

Avoid spring route motion when:

- The app needs strict platform-default transitions.
- The route contains dense data where wobble distracts.
- Reduced-motion users should receive a simpler transition.

## Presets

If hand tuning is too slow, the motor package ships presets matching platform design systems:

    SingleMotionBuilder(
      motion: CupertinoMotion.bouncy(),
      value: target,
      builder: (context, v, _) {
        return Transform.translate(
          offset: Offset(v, 0),
          child: child,
        );
      },
    );

    SingleMotionBuilder(
      motion: MaterialSpringMotion.expressiveSpatialDefault(),
      value: target,
      builder: (context, v, _) {
        return Transform.translate(
          offset: Offset(v, 0),
          child: child,
        );
      },
    );

MaterialSpringMotion splits spatial motion, such as position and size, from effects motion, such as opacity and color. Each has standard and expressive variants at fast, default, and slow speeds. Match the platform being emulated, or mix intentionally.

## Review checklist

- Does the spring preserve gesture velocity where the user released an object?
- Is overshoot visible where intended, or did a bounded controller clamp it away?
- Does damping stop the motion before it feels distracting?
- Does stiffness match the object scale and visual density?
- Does the same spring feel good on low-end devices and high-refresh screens?
- Is there a reduced-motion fallback when spring movement is prominent?
