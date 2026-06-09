# Icon Precaching

Use this reference when Flutter icons or first-screen assets pop in one or two frames late.

Sources:

- https://flutterpro.design/details/precache-icons
- https://pub.dev/packages/flutter_svg

## Problem

Flutter loads and decodes image assets when a widget first asks for them. If critical first-screen icons are not cached, they can pop in after the surrounding UI, which makes the app feel unfinished.

## Helper

For SVGs, use flutter_svg's loader cache. For raster assets, use precacheImage:

    import 'package:flutter_svg/flutter_svg.dart';

    Future<void> precacheAssets({
      required BuildContext context,
      required List<String> paths,
    }) async {
      for (final String path in paths) {
        if (path.endsWith('.svg')) {
          final SvgAssetLoader loader = SvgAssetLoader(path);

          await svg.cache.putIfAbsent(
            loader.cacheKey(null),
            () => loader.loadBytes(null),
          );
        } else {
          await precacheImage(AssetImage(path), context);
        }
      }
    }

## Splash split

Preload only visible-at-start assets before the first real screen:

    Future<void> preloadAssets(BuildContext context) async {
      await precacheAssets(
        context: context,
        paths: [
          'assets/icons/nav-bar/home.svg',
          'assets/icons/nav-bar/search.svg',
          'assets/icons/nav-bar/profile.svg',
          'assets/images/logo.png',
        ],
      );

      precacheAssets(
        context: context,
        paths: [
          'assets/icons/account/settings.svg',
          'assets/icons/account/notifications.svg',
        ],
      );
    }

If the repo uses asset codegen such as flutter_gen, prefer generated asset lists over manual string paths.

## Review checklist

- Are first-screen icons and logo assets cached during splash or bootstrap?
- Are offscreen assets loaded in the background rather than blocking startup?
- Are SVG and raster assets handled with the right cache path?
- Does the splash itself avoid uncached icons that pop in?
- Is the preload list small enough not to slow first paint?
