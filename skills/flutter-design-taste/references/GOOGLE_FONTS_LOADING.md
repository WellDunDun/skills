# Google Fonts Loading

Use this reference when a Flutter app uses the google_fonts package and text briefly appears in the default font before swapping to the intended typeface.

Sources:

- https://flutterpro.design/details/google-fonts-glitch
- https://pub.dev/packages/google_fonts

## Problem

The google_fonts package can download fonts the first time they are used. On a fresh install, users may see text render in the default font for a split second, then swap to the chosen font. That font flash weakens first impression and can create visible layout movement.

## Fix

Use GoogleFonts.pendingFonts to preload critical fonts before showing the app's main UI. Await the fonts during splash or bootstrap, then render the first real screen with the intended typography from frame one.

    Future<void> preloadFonts() async {
      await GoogleFonts.pendingFonts([
        GoogleFonts.inter(fontWeight: FontWeight.w400),
        GoogleFonts.inter(fontWeight: FontWeight.w700),
      ]);

      _preloadOtherFonts();
    }

    Future<void> _preloadOtherFonts() async {
      await GoogleFonts.pendingFonts([
        GoogleFonts.inter(fontWeight: FontWeight.w600),
      ]);
    }

The first await should include only fonts needed immediately for the first meaningful screen. Load secondary weights and styles in the background so startup does not wait for typography that is not yet visible.

## Exact weight and style

GoogleFonts.inter() does not preload every Inter variant. It loads the default normal 400 style. Preload the exact weights and styles used by visible text:

- Body using regular: w400 normal.
- Heading using bold: w700 normal.
- Caption using italic: w400 italic.
- Section label using semibold: w600 normal.

If a weight or style appears above the fold and was not preloaded, that text can still flash or swap later.

## Bootstrap pattern

Keep font preloading near the app bootstrap or splash state:

    class AppBootstrap extends StatefulWidget {
      const AppBootstrap({super.key});

      @override
      State<AppBootstrap> createState() => _AppBootstrapState();
    }

    class _AppBootstrapState extends State<AppBootstrap> {
      late final Future<void> _ready = preloadFonts();

      @override
      Widget build(BuildContext context) {
        return FutureBuilder<void>(
          future: _ready,
          builder: (context, snapshot) {
            if (snapshot.connectionState != ConnectionState.done) {
              return const SplashScreen();
            }

            return const AppShell();
          },
        );
      }
    }

Use the repo's existing splash/bootstrap pattern if it has one. Do not invent a new app shell just for font loading.

## Review checklist

- Does the first meaningful screen wait for critical font weights and styles?
- Are only above-the-fold fonts blocking startup?
- Are less urgent weights/styles preloaded in the background?
- Are italic, bold, semibold, and other non-default styles explicitly preloaded when used?
- Does the splash screen avoid using a not-yet-loaded font variant?
- Has a fresh install or cleared-cache run been checked for font flash?
