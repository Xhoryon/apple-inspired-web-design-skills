# UIKit / AppKit / QuickLook Media

Verified APIs from `BATCH8-APPLE-API-SYMBOL-INVENTORY.md` (DocC + installed MacOSX26.5 SDK).

## UIKit

### `UIImageView`

`UIImageView` (iOS 2.0+ / iPadOS 2.0+ / Mac Catalyst 13.0+ / tvOS 9.0+) — image display view.

`ContentMode` enum:
- `.scaleToFill`
- `.scaleAspectFit`
- `.scaleAspectFill`
- `.redraw`
- `.center` / `.top` / `.bottom` / `.left` / `.right`
- `.topLeft` / `.topRight` / `.bottomLeft` / `.bottomRight`

### `UIBezierPath` (for 2.5D layered composition)

`UIBezierPath` (iOS 3.2+ / iPadOS 3.2+ / Mac Catalyst 13.0+ / tvOS 9.0+) — path drawing; commonly used for 2.5D layered compositions.

### `UIVisualEffectView` (for 2.5D layered composition)

`UIVisualEffectView` (iOS 8.0+ / iPadOS 8.0+ / Mac Catalyst 13.0+ / tvOS 9.0+) with `UIBlurEffect` — visual effects (blur / vibrancy) for media composition.

Conditional — use for 2.5D layering effects; do NOT over-rely.

## AppKit

### `NSImageView`

`NSImageView` (macOS 10.0+) — image display view.

`ImageAlignment` + `ImageScaling` enums (macOS 10.0+).

### `CAAnimation` (Core Animation)

`CAAnimation` (QuartzCore, macOS 10.5+ / iOS 2.0+) — animation primitive. Used for camera-like motion.

The Skill does NOT invent Apple-canonical cinematic primitives. Camera-like motion vocabulary (approach / orbit / pan / tilt / scale / reveal) is **Skill-owned Layer C**, NOT Apple canonical.

## QuickLook (system preview)

### `QLPreviewController` (UIKit)

`QLPreviewController` (iOS 4.0+ / iPadOS 4.0+ / Mac Catalyst 13.0+ / visionOS 1.0+) — system preview.

### `QLPreviewPanel` (AppKit)

`QLPreviewPanel` (macOS 10.5+) — system preview panel.

### When to use system preview

- Generic file preview.
- User task is quick confirmation without entering working context.
- Platform support is available.

Do NOT use when:
- Custom inspection is required.
- Specialized media behavior is core.
- Domain interaction cannot be expressed through generic preview.

System primitive first, not system primitive always.

## Reference

- `references/swiftui-native-media.md` — SwiftUI media
- `references/media-role-architecture.md` — Media Role Contract
- `references/application-media-modes.md` — Browse / Preview / Inspect / Compare / Work