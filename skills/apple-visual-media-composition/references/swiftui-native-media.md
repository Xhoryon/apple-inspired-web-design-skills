# SwiftUI Native Media

Verified APIs from `BATCH8-APPLE-API-SYMBOL-INVENTORY.md` (DocC + installed MacOSX26.5 swiftinterface).

## `Image`

SwiftUI `Image` (iOS 13+ / iPadOS 13+ / Mac Catalyst 13+ / macOS 10.15+ / tvOS 13+ / watchOS 6+ / visionOS 1.0+) — image rendering.

Aspect / sizing modifiers:
- `.resizable()`
- `.aspectRatio(_:contentMode:)` (`.fit` / `.fill`)
- `.scaledToFit()` / `.scaledToFill()`

Use system image modifiers; do NOT re-implement aspect / scaling.

## `AsyncImage`

SwiftUI `AsyncImage` (iOS 15+ / iPadOS 15+ / Mac Catalyst 15+ / macOS 12+ / tvOS 15+ / watchOS 8+ / visionOS 1.0+) — async image loading with `AsyncImagePhase` (`.empty` / `.success(Image)` / `.failure(Error)`).

Use for progressive image loading. Coordinate with `apple-feedback-response` for failure status.

## `.quickLookPreview(_:)`

SwiftUI `.quickLookPreview(_:)` (iOS 14+ / iPadOS 14+ / Mac Catalyst 14+ / tvOS 14+ / watchOS 7+ / visionOS 1.0+) — system preview integration.

iOS family only. NOT native macOS (use `QLPreviewPanel` on AppKit).

## `VideoPlayer`

SwiftUI `VideoPlayer` (iOS 14+ / iPadOS 14+ / Mac Catalyst 14+ / macOS 11+ / tvOS 14+ / watchOS 7+ / visionOS 1.0+) — video presentation.

iOS + macOS + tvOS + watchOS + visionOS.

## `RealityView`

SwiftUI `RealityView` (in `_RealityKit_SwiftUI` framework; visionOS 1.0+ / iOS 18+ / iPadOS 18+ / Mac Catalyst 18+) — 3D model view.

Per-platform availability preserved. NOT exported in MacOSX26.5 macabi swiftinterface slice in the current SDK build.

## Reference

- `references/uikit-appkit-quicklook-media.md` — UIKit / AppKit media
- `references/media-role-architecture.md` — Media Role Contract
- `references/media-loading-failure.md` — loading / failure