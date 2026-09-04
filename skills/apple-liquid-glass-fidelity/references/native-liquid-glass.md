# Native Liquid Glass Reference

The native Apple Liquid Glass API surface and the system-primitive-first routing. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

The native APIs are owned by Apple. The Skill records the documented APIs and the routing rule: on supported Apple platforms, the system primitive is the correct answer. The Web should not attempt to re-implement the native behavior; it should approximate the intent.

## Framework × Symbol × Platform availability (canonical evidence table)

Evidence for this table comes from the **installed macOS 26.5 SDK** (`MacOSX26.5.sdk`) headers and `.swiftinterface` files, and from Apple's live DocC on `developer.apple.com` (verified via `curl` during this remediation; SDK and DocC agree).

| Framework | Symbol | Purpose | iOS | iPadOS | Mac Catalyst | macOS | tvOS | watchOS | visionOS | Maturity | Evidence |
|---|---|---|---|---|---|---|---|---|---|---|---|
| SwiftUI | `Glass` (struct) | Material descriptor; `.regular` (default), `.clear`, `.identity`; `.interactive(Bool)` chained off the value. | 26.0 | 26.0 | (via iOS) | 26.0 | 26.0 | 26.0 | **unavailable** | Stable (SwiftUICore `.swiftinterface`: `@available(iOS 26.0, macOS 26.0, tvOS 26.0, watchOS 26.0, *) @available(visionOS, unavailable)`) | `SwiftUICore.swiftinterface` arm64e-apple-macos / arm64e-apple-ios-macabi |
| SwiftUI | `DefaultGlassEffectShape` | The default shape applied by `glassEffect(_:in:)`; a capsule. | 26.0 | 26.0 | (via iOS) | 26.0 | 26.0 | 26.0 | **unavailable** | Stable | `SwiftUICore.swiftinterface` |
| SwiftUI | `View.glassEffect(_:in:)` | Apply a Liquid Glass material to a view. Default signature: `func glassEffect(_ glass: Glass = .regular, in shape: some Shape = DefaultGlassEffectShape()) -> some View`. | 26.0 | 26.0 | (via iOS) | 26.0 | 26.0 | 26.0 | **unavailable** | Stable | `SwiftUICore.swiftinterface` |
| SwiftUI | `View.glassEffectID(_:in:)` | Tag a view with a glass-effect identity so it joins a unified effect inside a `GlassEffectContainer`. | 26.0 | 26.0 | (via iOS) | 26.0 | 26.0 | 26.0 | **unavailable** | Stable | `SwiftUICore.swiftinterface` |
| SwiftUI | `View.glassEffectTransition(_:)` | Configure the transition for a view with a `glassEffectID`. Default: `.matchedGeometry`. Available transitions: `matchedGeometry`, `materialize`, `identity`. | 26.0 | 26.0 | (via iOS) | 26.0 | 26.0 | 26.0 | **unavailable** | Stable | `SwiftUICore.swiftinterface` |
| SwiftUI | `GlassEffectContainer` | Container view that shares a sampling region across multiple glass children and enables morph between them via `glassEffectID`. Configurable `spacing` parameter. | 26.0 | 26.0 | (via iOS) | 26.0 | 26.0 | 26.0 | **unavailable** | Stable | `SwiftUICore.swiftinterface` |
| UIKit | `UIGlassEffect` | `UIVisualEffect` subclass. `+ effectWithStyle:`, `interactive` (Bool), `tintColor`. | 26.0 | 26.0 | (via iOS) | **not available** | (UIKit ships on tvOS but header does not list this API; DocC lists tvOS for `UIGlassContainerEffect`; UIKit tvOS support for `UIGlassEffect` itself is **UNVERIFIED** in current evidence) | **unavailable** (`API_UNAVAILABLE(visionos, watchos)` in the iOS-family header) | Stable | `UIGlassEffect.h` (iOSSupport) |
| UIKit | `UIGlassContainerEffect` | `UIVisualEffect` subclass. `spacing` (CGFloat). When nested in a `UIVisualEffectView`'s `contentView` with multiple `UIGlassEffect` instances, renders all glass elements in one combined view. | 26.0 | 26.0 | (via iOS) | **not available** (the AppKit class `NSGlassEffectContainerView` is the macOS-native surface; UIKit/Mac Catalyst inherits `UIGlassContainerEffect`) | DocC lists tvOS — see UNVERIFIED note | **unavailable** | Stable | `UIGlassEffect.h` (iOSSupport) |
| UIKit | `UIGlassEffectTransition` | **DOES NOT EXIST.** No header in the installed SDK; DocC returns HTTP 404. The Skill records this as UNSUPPORTED. | n/a | n/a | n/a | n/a | n/a | n/a | n/a | **UNSUPPORTED** | `grep -r UIGlassEffectTransition MacOSX26.5.sdk` → 0 occurrences |
| AppKit | `NSGlassEffectView` (NSView subclass) | A view that embeds its `contentView` in a Liquid Glass material. Properties: `contentView`, `cornerRadius`, `tintColor`, `style` (`NSGlassEffectViewStyleRegular` / `NSGlassEffectViewStyleClear`). | n/a | n/a | **unavailable on Mac Catalyst** (`APPKIT_API_UNAVAILABLE_BEGIN_MACCATALYST`) | **26.0+** (`API_AVAILABLE(macos(26.0))`, `beta:false`, `unavailable:false` per DocC) | n/a | n/a | n/a | **Stable** (not Beta, not preliminary, not Early Access) | `NSGlassEffectView.h`; `AppKit.tbd`; Apple DocC |
| AppKit | `NSGlassEffectContainerView` (NSView subclass) | Container view that efficiently merges descendant `NSGlassEffectView`s when within `spacing` proximity. | n/a | n/a | **unavailable on Mac Catalyst** | **26.0+** (`API_AVAILABLE(macos(26.0))`, `beta:false`, `unavailable:false` per DocC) | n/a | n/a | n/a | **Stable** | `NSGlassEffectView.h`; Apple DocC |
| AppKit | `NSGlassEffectView.effectIsInteractive` | Whether the glass surface responds to user interaction. | n/a | n/a | (not in 26.5 SDK; see version) | **27.0+** (DocC: `introducedAt:"27.0"`, `beta:true`). **Not present in the macOS 26.5 SDK** at all. | n/a | n/a | n/a | **VERSION-SPECIFIC / BETA** | Apple DocC `nsglasseffectview/effectisinteractive.json` |
| visionOS glass | `glassBackgroundEffect` and related | visionOS's own SwiftUI glass-background system, distinct from the new Liquid Glass API family. | n/a | n/a | n/a | n/a | n/a | n/a | (visionOS) | Stable | Apple Developer visionOS materials |

Notes on the table:

- **visionOS**: the new SwiftUI Liquid Glass family (`Glass` / `glassEffect` / `GlassEffectContainer` / `glassEffectTransition` / `DefaultGlassEffectShape`) is **NOT available** on visionOS. All five symbols carry `@available(visionOS, unavailable)` in the SDK. visionOS has its own glass-background system (`glassBackgroundEffect` etc.) which is a separate API family and out of scope for this Skill.
- **Mac Catalyst**: UIKit glass (iOS-family) is available on Mac Catalyst because UIKit-on-Mac-Catalyst inherits UIKit's iOS availability. AppKit glass is **not available** on Mac Catalyst (`APPKIT_API_UNAVAILABLE_BEGIN_MACCATALYST`).
- **UIKit on macOS**: `UIGlassEffect` / `UIGlassContainerEffect` are iOS-family APIs; they are not declared for native macOS. The macOS-native glass APIs are the AppKit classes. The Skill does NOT claim that `UIGlassEffect` works on native macOS.
- **UIKit on tvOS**: `UIGlassContainerEffect` is listed for tvOS in Apple DocC; `UIGlassEffect` tvOS support is not annotated in the iOSSupport header (the iOSSupport header does not list tvOS as available; DocC listing for `UIGlassEffect` tvOS is **UNVERIFIED** in current evidence). Recorded honestly; the implementer consults current Apple documentation.
- **AppKit `effectIsInteractive`**: absent from the macOS 26.5 SDK. Apple DocC records it as Beta, introduced at macOS **27.0** (not 26.0). The earlier development text that listed it as Beta in macOS 26.0 was incorrect.
- **`UIGlassEffectTransition`**: not present in the installed SDK; DocC returns 404. The earlier development text that listed it as a UIKit API was incorrect. The Skill records this as **UNSUPPORTED** — there is no UIKit transition primitive that mirrors SwiftUI's `glassEffectTransition`.

## Status model (per-symbol, narrowest verified truth)

The Skill records availability **per symbol**, not as a single umbrella verdict:

| Symbol | Status | Rationale |
|---|---|---|
| `NSGlassEffectView` | **Stable from macOS 26.0** | DocC `beta:false`, `unavailable:false`; SDK `API_AVAILABLE(macos(26.0))`. |
| `NSGlassEffectContainerView` | **Stable from macOS 26.0** | Same as above. |
| `NSGlassEffectView.effectIsInteractive` | **VERSION-SPECIFIC / BETA, macOS 27.0+** | DocC `beta:true`, `introducedAt:"27.0"`; absent from the macOS 26.5 SDK. |
| `UIGlassEffect` | **Stable from iOS 26.0 / iPadOS 26.0 / Mac Catalyst** | iOSSupport header `API_AVAILABLE(ios(26.0))`; AppKit `NSGlassEffectView` is the native macOS surface, not `UIGlassEffect`. |
| `UIGlassContainerEffect` | **Stable from iOS 26.0 / iPadOS 26.0 / Mac Catalyst** | Same as above. tvOS listed in DocC; precise header not annotated. |
| `UIGlassEffectTransition` | **DOES NOT EXIST — UNSUPPORTED.** | SDK 0 occurrences; DocC 404. |
| `Glass`, `glassEffect`, `GlassEffectContainer`, `glassEffectID`, `glassEffectTransition`, `DefaultGlassEffectShape` (SwiftUI family) | **Stable from iOS 26.0 / iPadOS 26.0 / Mac Catalyst / macOS 26.0 / tvOS 26.0 / watchOS 26.0** | `SwiftUICore.swiftinterface` `@available(iOS 26.0, macOS 26.0, tvOS 26.0, watchOS 26.0, *) @available(visionOS, unavailable)`. |
| `NS_UNAVAILABLE_FOR_USE_IN_STABLE_APPS` on AppKit glass | **Does not exist in the installed SDK or Apple DocC.** | `grep -r "NS_UNAVAILABLE_FOR_USE_IN_STABLE_APPS" MacOSX26.5.sdk` → 0 occurrences. The previously recorded claim was fabricated. |
| Early Access Releases requirement for AppKit glass | **Not required.** AppKit glass classes are stable shipping APIs from macOS 26.0. | DocC; SDK headers. |

## AppKit container semantics

`NSGlassEffectContainerView` is documented as efficiently merging descendant `NSGlassEffectView`s when they are within a configured proximity (`spacing` parameter). Documented behaviors:

- Proximity-based grouping / merging of descendant glass effect views.
- Batched processing of similar glass views to improve rendering performance.
- Z-order elevation of descendants of `contentView` so they appear above the `contentView`.

The Skill does NOT claim that `NSGlassEffectContainerView` performs the same morphing semantics as SwiftUI `GlassEffectContainer`. The SwiftUI `glassEffectID` + `glassEffectTransition` morphing primitive is a SwiftUI feature; the AppKit container's merge is documented as proximity-based performance grouping. Framework-specific behavior is kept separate.

## SwiftUI signature detail

The actual SwiftUI signature:

```swift
func glassEffect(
    _ glass: Glass = .regular,
    in shape: some Shape = DefaultGlassEffectShape()
) -> some View
```

Interactivity is enabled by chaining `.interactive()` off the `Glass` value:

```swift
.glassEffect(.regular.interactive(), in: ...)
```

The Skill does NOT publish a separate `isInteractive:` argument. The earlier development text that listed `View.glassEffect(_:in:isInteractive:)` as the signature was incorrect; the correct API is the chained `.interactive()` method on the `Glass` instance.

## SwiftUI transition identity model

`GlassEffectTransition` has exactly three documented members:

- `matchedGeometry` (default).
- `materialize`.
- `identity`.

These are the only values SwiftUI accepts for `glassEffectTransition(...)`. The Skill does NOT publish other transition names (no `dematerialize`, no `merge`, no `separate`, no `reshape`, no `retarget`); the Skill's own morph model (`materialize / dematerialize / merge / separate / reshape / retarget`) is the Skill's design vocabulary, not a SwiftUI API mapping. See `glass-morphing.md` for the morph model.

## Regular vs Clear variants

The native Apple API offers two variants. The Skill records the principle; the exact API surface is owned by Apple documentation.

- **Regular** — the default; legibility first; stronger background adaptation; more visible edge. Used for toolbars, tab bars, segmented controls, sheets, popovers, and any surface where the user reads text or interacts with controls.
- **Clear** — high translucency; underlying media visibility; restrained foreground content. Used for media overlays, image-caption bars, and the rare surface where the point is to see through it.

The implementer chooses the variant based on the design intent. The wrong variant is a defect.

### Apple's three-condition rule for Clear

A Clear variant is appropriate **only when ALL THREE of the following conditions hold simultaneously**:

1. The Liquid Glass element is positioned over **media-rich content** (photograph, illustration, video, map, or other visually dense backdrop).
2. Introducing the required **dimming layer** does not negatively harm the content layer beneath the glass.
3. The **foreground content above the glass is bold and bright** enough to remain legible against the see-through + dimming effect.

Decision logic for Clear:

```text
Clear candidate
  → condition 1 (over media-rich content)?
  → condition 2 (dimming layer does not harm content)?
  → condition 3 (foreground is bold and bright)?
  → all three yes: potentially appropriate
  → any condition no: use Regular / another material / adapt
```

Clear is NOT simply "more transparent Regular." Clear is a distinct variant with its own constraints. The Skill preserves this rule.

### Do not mix Regular and Clear within one design treatment

The Skill preserves Apple's literal rule: **Never mix Regular and Clear in the same interface / design treatment.** Choose one variant and stick with it for visual consistency.

On the Web, the equivalent rule is: **Do not mix Regular-like and Clear-like within one design treatment.** The Skill does NOT weaken this rule to "avoid mixing when possible." It is preserved.

The rule applies at every level of the surface: in a single surface, in a control cluster (segmented / shared highlight), in a sheet, and across a design treatment. Selection within a single Regular-like cluster is expressed through **tint, foreground, border, geometry, or shared highlight**, not through mixing in a Clear-like variant. See `material-fidelity.md` §"Selection-state techniques within one material family" for the canonical list.

If a future Apple source revises this guidance, the rule is updated by source-matrix revision; until then, it stands.

## The system-primitive-first rule

On supported Apple platforms, the system primitive is the correct answer. The Skill does NOT recommend custom re-implementations of Liquid Glass on native Apple platforms. The reasons:

- The system primitive is updated with each OS release. A custom re-implementation is a maintenance burden.
- The system primitive is accessibility-aware (Reduce Transparency, Increase Contrast, Reduce Motion).
- The system primitive is performance-aware (compositor, GPU, multi-surface sampling).
- The system primitive is consistency-aware (looks the same across the OS).

The Web is a different story. The Web has no `glassEffect()`; the Web must approximate. The approximation is honest: it is a Web approximation, not a port.

## When to use the native API

Use the native API when:

- The target is a supported Apple platform (iOS 26.0+ / iPadOS 26.0+ / Mac Catalyst / macOS 26.0+ / tvOS 26.0+ / watchOS 26.0+ for SwiftUI; iOS / iPadOS / Mac Catalyst for UIKit glass; macOS 26.0+ for AppKit glass).
- The design intent matches the documented behavior.
- For AppKit glass on macOS: stable shipping APIs are available from macOS 26.0. The `effectIsInteractive` property is macOS 27.0+ and Beta; do NOT assume it is shippable today.

Use the native API **even if** the design is on Web and the user is on a Native Apple browser. The browser does not expose the native API to Web content; this is a technical limitation, not a design choice. The Skill records the principle: Web content cannot call `glassEffect()` on a Native Apple browser.

## When the native API is not available

On earlier OS versions (iOS ≤ 25 / iPadOS ≤ 25 / macOS ≤ 25 / tvOS ≤ 25 / watchOS ≤ 25), the SwiftUI Liquid Glass family is not available. The Skill records:

- The user sees the pre-Liquid-Glass material behavior on those OS versions.
- The Web must not assume the new behavior; it should test on the actual target.
- The implementer may use the stable v1.1 `apple-liquid-glass-web` Web approximation as a fallback.

visionOS has its own glass-background system (`glassBackgroundEffect` etc.). The new SwiftUI Liquid Glass API family is not available on visionOS. Vision-OS surfaces use the visionOS glass-background system or a standard material.

## What the Skill rejects

- A custom re-implementation of Liquid Glass on Native Apple platforms (rejected — use the system primitive).
- A claim that the Web has equivalent sampling / morphing capability (rejected).
- A claim that the Web can call `glassEffect()` (rejected).
- A claim that the new SwiftUI Liquid Glass family is available on visionOS (rejected — `@available(visionOS, unavailable)`).
- A claim that `UIGlassEffect` is the native macOS glass API (rejected — `UIGlassEffect` is iOS-family; the macOS-native class is `NSGlassEffectView`).
- A claim that `UIGlassEffectTransition` exists (rejected — UNSUPPORTED; SDK 0 occurrences; DocC 404).
- A claim that AppKit glass on macOS is preliminary / early-access / `NS_UNAVAILABLE_FOR_USE_IN_STABLE_APPS` (rejected — fabricated; the SDK and DocC record AppKit glass classes as `beta:false`, `unavailable:false`, `API_AVAILABLE(macos(26.0))`).
- A claim that any of the rejected optical properties (chromatic aberration, DOF, anisotropy, caustics, lens dispersion, Fresnel) are required by Apple (rejected — UNSUPPORTED).
- A claim that the SwiftUI signature has an `isInteractive:` parameter (rejected — interactivity is via `.interactive()` chained off the `Glass` value).
- A claim that `effectIsInteractive` is macOS 26.0 (rejected — DocC `introducedAt:"27.0"`; absent from the macOS 26.5 SDK).

## Cross-Skill note

- The native APIs are owned by Apple; the Skill recommends the system primitive.
- The state model is `apple-control-states`; the Skill describes how the material responds, not what the state is.
- The motion physics are `apple-motion-physics`.
- The legibility model is `glass-legibility.md`.
- The Web approximation is in `web-capability-fallback.md` and the stable v1.1 `apple-liquid-glass-web` baseline.
- The source citations are in `APPLE-EXPERIENCE-LIQUID-GLASS-SOURCE-MATRIX.md`.