# Image Loading

Use this reference when Flutter images should load smoothly instead of popping into place with no transition.

Sources:

- https://flutterpro.design/details/smooth-image-loading
- https://pub.dev/packages/image_fade

## Problem

Default image rendering can feel abrupt: the image area is empty or placeholder-like, then the final image suddenly appears. In image-heavy surfaces, this makes the app feel less polished and can draw attention away from the content.

## Use image_fade

The image_fade package shows a placeholder while loading, then cross-fades to the loaded image or an error widget.

    ImageFade(
      image: NetworkImage(
        'https://example.com/image.png',
      ),
      placeholder: Container(
        color: Colors.grey.shade100,
      ),
      errorBuilder: (context, exception) {
        return Container(
          color: Colors.grey.shade100,
          child: Icon(
            Icons.error_outline,
            size: 20,
            color: Colors.grey.shade400,
          ),
        );
      },
    )

The image argument accepts any ImageProvider, including:

- NetworkImage
- AssetImage
- CachedNetworkImageProvider
- ExtendedNetworkImageProvider
- Other custom ImageProvider implementations

## Placeholder taste

Keep placeholders simple:

- Solid light neutral fill.
- Subtle shimmer when the product already uses skeleton loading.
- Reserved aspect-ratio box matching the final image.

Avoid:

- Spinners or progress indicators for ordinary images.
- Loud branded placeholders repeated in grids.
- Layout that changes size when the image loads.

## Error states

Image errors should be quiet unless the image is the main object of the task.

Use:

- Same background as placeholder.
- Small muted icon.
- Optional retry affordance only when the user can reasonably recover.

Avoid:

- Technical error messages.
- Large warning UI in repeated image grids.
- Bright destructive colors for routine missing thumbnails.

## Sizing and fit

Use stable image dimensions:

    ImageFade(
      image: NetworkImage(url),
      width: 96,
      height: 96,
      fit: BoxFit.cover,
      placeholder: DecoratedBox(
        decoration: BoxDecoration(
          color: Colors.grey.shade100,
        ),
      ),
      errorBuilder: (context, exception) => const _ImageError(),
    )

Check the package docs for width, height, fit, duration, syncDuration, and related options.

## Review checklist

- Does the image reserve the same size before and after loading?
- Is the placeholder quiet enough for repeated lists and grids?
- Does the image fade in instead of popping?
- Is the error state subtle and non-technical?
- Is the ImageProvider compatible with existing caching or network image strategy?
- Are large source images sized appropriately for their displayed dimensions?
- Are above-the-fold icons and image assets precached when they otherwise pop in? See [ICON_PRECACHE.md](ICON_PRECACHE.md).
