# Media Loading & Failure

## Loading / success / failure states

- SwiftUI `AsyncImage` (iOS 15+ / iPadOS 15+ / Mac Catalyst 15+ / macOS 12+ / tvOS 15+ / watchOS 8+ / visionOS 1.0+): `AsyncImagePhase` enum — `.empty` / `.success(Image)` / `.failure(Error)`.
- UIKit: `UIImageView` + `loadImage` / `UIImage` async loading; placeholder images.
- AppKit: `NSImageView` + `loadImage` async loading.
- Web: HTML `Image()` element + `onload` / `onerror` events; W3C `IntersectionObserver` for lazy loading.

## Loading placeholders

Do not universally use:
- skeleton shimmer
- blurhash
- gradient placeholder

Choose based on:
- expected latency
- content role
- known aspect
- visual stability

No decorative loading template. State what the placeholder represents.

## Hard principle: loading must not destroy continuity

- Avoid aspect ratio jump (reserve `aspect-ratio` in CSS; known image dimensions).
- Avoid selection / zoom / pan / caret loss during image swap.
- Avoid misleading "success" state (image visible but not yet loaded).
- Avoid layout shift that breaks interactive context.

## Failure state

- Show a meaningful error / placeholder.
- Offer retry where reasonable.
- Do not silently keep a spinner.

## Progressive image loading

Reserve aspect ratio via:
- CSS `aspect-ratio`
- HTML `width` / `height` attributes
- SwiftUI `aspectRatio(_:contentMode:)`

Use `IntersectionObserver` for lazy loading.

## Reference

- `references/web-responsive-cinematic-media.md` — Web primitives
- `references/media-accessibility.md` — accessibility
- `references/professional-media-workflow.md` — workflow