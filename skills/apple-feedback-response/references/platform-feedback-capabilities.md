# Platform Feedback Capabilities

Per-platform feedback capability matrix. The Skill records what each platform supports; the Skill does NOT assume parity.

## Apple platforms

### iOS / iPadOS (iOS 13+)

| Capability | Available | Notes |
|---|---|---|
| SwiftUI `SensoryFeedback` | iOS 17+ | per-pattern playback varies |
| UIKit `UIFeedbackGenerator` family | iOS 10+ | full set |
| `UICanvasFeedbackGenerator` | iOS 17.5+ | Apple Pencil alignment / path completion |
| `UndoManager` (Foundation) | iOS 13+ | cross-platform; shake-to-undo on iPhone (iOS 9+) |
| `ProgressView` (SwiftUI) | iOS 14+ | |
| `UIProgressView` (UIKit) | iOS 2.0+ (legacy) | |
| AudioServicesPlaySystemSound | iOS 9+ (deprecated for non-system use) | for system sounds |
| Accessibility media queries | iOS 9+ (prefers-reduced-motion); iOS 10+ (prefers-reduced-transparency); iOS 13+ (prefers-contrast) | |

### macOS (macOS 11+)

| Capability | Available | Notes |
|---|---|---|
| SwiftUI `SensoryFeedback` | macOS 14+ | per-pattern playback varies |
| AppKit `NSHapticFeedbackManager` | macOS 10.10+ | trackpad-dependent |
| `NSUndoManager` (AppKit) | macOS 10.0+ | menu-bar driven |
| `UndoManager` (Foundation) | macOS 10.15+ | cross-platform |
| `ProgressView` (SwiftUI) | macOS 11+ | |
| `NSProgressIndicator` (AppKit) | macOS 10.0+ | |
| AudioServicesPlaySystemSound | macOS 10.12+ | for system sounds |
| `NSSound` | macOS 10.0+ | native Mac sound playback |
| Accessibility media queries | macOS 10.12+ | |

### visionOS

| Capability | Available | Notes |
|---|---|---|
| SwiftUI `SensoryFeedback` | visionOS 1.0+ (per-pattern playback varies) | spatial feedback |
| UIKit `UIFeedbackGenerator` family | NOT first-class on visionOS; UIKit is iOS-family | visionOS has its own spatial feedback system |
| `UICanvasFeedbackGenerator` | NOT on visionOS (per installed header `API_UNAVAILABLE(visionos)`) | |
| `UndoManager` (Foundation) | visionOS 1.0+ | cross-platform |
| `ProgressView` (SwiftUI) | visionOS 1.0+ | |
| AudioServicesPlaySystemSound | visionOS 1.0+ | |

### tvOS

| Capability | Available | Notes |
|---|---|---|
| SwiftUI `SensoryFeedback` | tvOS 17+ (per-pattern playback varies) | |
| UIKit `UIFeedbackGenerator` family | NOT first-class on tvOS | |
| `UndoManager` (Foundation) | tvOS 13+ | limited UI; focused-device |
| `ProgressView` (SwiftUI) | tvOS 14+ | |

### watchOS

| Capability | Available | Notes |
|---|---|---|
| `WKInterfaceDevice.current().play(_:)` | watchOS 2.0+ | hardware-dependent playback |
| SwiftUI `SensoryFeedback` | watchOS 10+ | per-pattern playback varies |
| `UndoManager` (Foundation) | watchOS 6.0+ | limited; Digital Crown is the primary input |
| `ProgressView` (SwiftUI) | watchOS 7+ | platform-specific HIG guidance applies |
| Haptic Engine | Apple Watch hardware | per-device |

### Core Haptics (iOS / iPadOS / Mac Catalyst / visionOS)

| Capability | Available | Notes |
|---|---|---|
| `CHHapticEngine` | iOS 13+ / iPadOS 13+ / Mac Catalyst 13.0+ / visionOS 1.0+ | custom-pattern API |
| `CHHapticPattern` | iOS 13+ | |
| `CHHapticEvent` | iOS 13+ | |
| `CHHapticDeviceCapability` | iOS 13+ | device capability query |

Core Haptics is NOT available on macOS native (AppKit uses `NSHapticFeedbackManager`), NOT on watchOS (Apple Watch uses `WKInterfaceDevice`), NOT on tvOS (Apple TV has no first-class haptic API).

## Web

| Capability | Available | Notes |
|---|---|---|
| `aria-live="polite"` / `aria-live="assertive"` | all modern browsers | live region for status / error |
| `<progress>` element + ARIA `progressbar` role | all modern browsers | accessible progress |
| `@media (prefers-reduced-motion: reduce)` | all modern browsers | reduced motion |
| `@media (prefers-reduced-transparency: reduce)` | all modern browsers (partial Safari support) | reduced transparency |
| `@media (prefers-contrast: more)` | all modern browsers (partial Safari support) | increased contrast |
| `navigator.vibrate(pattern)` | Android Chrome (full); Firefox (full); iOS Safari (limited / unreliable) | ASYMMETRIC |
| Web Audio API | all modern browsers | for custom audio |
| `<audio>` element | all modern browsers | for system sounds |

### Web haptic asymmetry

iOS Safari does NOT reliably support `navigator.vibrate`. The Skill explicitly records this asymmetry.

When the user is on iOS Safari and the feedback requires tactile reinforcement:

- The Skill falls back to visual + audio + text.
- The Skill does NOT attempt to detect device / viewport to claim "vibration works" — capability is asymmetric.

## Per-pattern playback availability

Symbol existence ≠ behavior parity. The Skill records:

- SwiftUI `SensoryFeedback` value available (iOS 17+, macOS 14+, ...) ≠ the value plays identically on every Apple platform.
- UIKit `UIFeedbackGenerator` available (iOS 10+) ≠ the generator produces identical feedback on every device.
- `UICanvasFeedbackGenerator` available (iOS 17.5+) ≠ the generator plays on visionOS / tvOS / watchOS — it does NOT (per installed header).

The Skill does NOT make per-pattern playback parity claims. Per-pattern playback requires per-pattern DocC verification.

## What the Skill rejects

- **Cross-platform parity assumptions** — assuming iPhone-style haptics on macOS / visionOS / tvOS / watchOS. Rejected.
- **Web = Taptic Engine** — assuming browser feedback prototype proves native iPhone haptic fidelity. Rejected.
- **Per-pattern playback parity** — assuming every `SensoryFeedback` value plays on every platform. Rejected.
- **Viewport-based capability detection** — using viewport width / user-agent to determine haptic capability. The Skill rejects this; capability must be detected via API.
- **Implicit Core Haptics** — using `CHHapticEngine` everywhere because it's powerful. The Skill labels Core Haptics as ADVANCED / CONDITIONAL.