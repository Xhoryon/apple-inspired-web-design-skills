# Application Media Modes

Five modes for how media appears within an application (vs marketing site). Skill-owned Layer C taxonomy; NOT Apple canonical.

## Browse

User task: find / recognize / navigate media.

Typical priorities:
- recognition
- density
- scanning
- thumbnail consistency
- selection / navigation
- metadata when useful

Do not make every Browse thumbnail a cinematic card.

## Preview

User task: quickly confirm identity / content without fully entering a working context.

System preview should be considered first where platform support makes sense:

- SwiftUI `.quickLookPreview(_:)` (iOS 14+ / iPadOS 14+ / Mac Catalyst 14+ / tvOS 14+ / watchOS 7+ / visionOS 1.0+).
- UIKit `QLPreviewController` (iOS 4.0+ / iPadOS 4.0+ / Mac Catalyst 13.0+ / visionOS 1.0+).
- AppKit `QLPreviewPanel` (macOS 10.5+).

Preview should NOT automatically become a full editing environment, complicated inspector, or giant cinematic transition. Preserve origin / context.

### System preview first — but not always

Use system preview primitives before building a custom previewer. But do NOT require Quick Look when:
- the product needs custom inspection
- specialized media behavior is core
- domain interaction cannot be expressed through generic preview

System primitive first, not system primitive always.

## Inspect

User task: carefully examine media content.

Media becomes dominant. Potential needs:
- zoom
- pan
- fit
- actual / meaningful size
- metadata
- navigation to adjacent media
- stable inspection state

`apple-visual-media-composition` owns inspection composition / state purpose. `apple-direct-manipulation` owns zoom / pan gesture mechanics.

## Compare

User task: understand differences.

Possible presentations:
- side-by-side
- synchronized viewport
- before / after
- overlay
- opacity comparison
- aligned detail inspection

Choose comparison composition based on task. Do not universally use a slider.

### Synchronized inspection

If comparing equivalent views: linked zoom / pan may be useful. If content differs structurally: forced synchronization may be harmful. Do not make synchronization universal.

## Work / Edit context

When media is the work object: the media canvas should dominate according to task needs.

Surrounding UI:
- toolbar
- sidebar
- inspector
- timeline
- metadata

should support work.

Avoid: **Marketing-Site-Inside-An-App syndrome.** No cinematic entrances for repeated core editing interactions unless genuinely task-serving.

## Zoom / pan state continuity

Where inspection / work requires stable viewport state:
- do not reset zoom / pan unnecessarily when sidebar appears, inspector changes, window resizes, metadata updates, related UI changes.
- Coordinate with `apple-adaptive-structure`.

## Reference

- `references/media-role-architecture.md` — Media Role Contract
- `references/interactive-media-explorer.md` — user state
- `references/hero-cinematic-storyboarding.md` — Hero choreography