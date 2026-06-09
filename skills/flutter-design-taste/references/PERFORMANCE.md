# Performance

Use this reference when UI polish risks jank, slow rebuilds, expensive layout, or memory-heavy rendering.

## Design-performance mindset

Good Flutter design must survive real runtime constraints. Shadows, blur, opacity, clipping, custom painting, large images, and animated layouts are design choices with rendering cost.

## Build cost

- Keep build() methods cheap.
- Move expensive computation out of build paths.
- Split large widgets by state-change boundary.
- Localize setState to the smallest subtree that changes.
- Prefer const constructors where possible.
- Prefer reusable components over helper methods returning repeated UI.

## Lists and grids

- Use lazy builders for long lists and grids.
- Avoid building every child up front.
- Provide stable keys for stateful rows.
- Reserve image aspect ratios and use placeholders to prevent layout jumps.
- Avoid intrinsic measurement in large repeated layouts.

## Rendering cost

- Avoid unnecessary Opacity, ShaderMask, ColorFilter, excessive clipping, and overlapping translucent layers.
- Use border radius properties instead of clipping where possible.
- Use RepaintBoundary around expensive independently repainting regions when profiling shows benefit.
- Be careful with blur and backdrop effects, especially in scrolling views.
- Avoid custom paint loops when a static asset or cached picture is enough.

## Images

- Size images close to displayed dimensions.
- Use placeholders and error states.
- Cross-fade loaded images and keep placeholders quiet. See [IMAGE_LOADING.md](IMAGE_LOADING.md).
- Precache only when it improves a near-future transition.
- Avoid loading full-resolution media into small thumbnails.

## Profiling

- Profile performance in profile mode, not debug mode.
- Use DevTools Performance view when jank appears.
- Track rebuilds and layout passes when the issue is not obvious.
- Test on the lowest-end device or simulator profile the product realistically targets.

## UI review questions

- Does this animation rebuild more than it needs to?
- Does this scroll surface build lazily?
- Does this layout require a costly intrinsic pass?
- Are opacity, clipping, and shadows necessary for the design?
- Does a skeleton, placeholder, or reserved size prevent layout jump?

## Sources

- Flutter performance best practices: https://docs.flutter.dev/perf/best-practices
- Flutter performance overview: https://docs.flutter.dev/perf
- Rendering performance: https://docs.flutter.dev/perf/rendering-performance
