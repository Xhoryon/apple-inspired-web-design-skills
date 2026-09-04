# Haptic Feedback

The Skill's haptic feedback model. Semantic mapping, system patterns vs custom, platform availability, accessibility, frequency / overuse.

## Hard invariant

**Critical information must not depend on haptics alone.** Haptics can complement visual / audio / text feedback, but must not be the only accessible channel for essential meaning.

## System-defined patterns (Apple-documented semantics)

The Skill uses Apple's documented semantic mappings. The Skill does NOT remap patterns.

### UIKit (`UIFeedbackGenerator` family)

| Pattern | Documented semantic | Available |
|---|---|---|
| `UIImpactFeedbackGenerator(style: .light)` | light impact (collision, drag threshold) | iOS 10+ |
| `UIImpactFeedbackGenerator(style: .medium)` | medium impact (collision, default) | iOS 10+ |
| `UIImpactFeedbackGenerator(style: .heavy)` | heavy impact (collision) | iOS 10+ |
| `UIImpactFeedbackGenerator(style: .soft)` | soft impact (collision, gentle) | iOS 13+ |
| `UIImpactFeedbackGenerator(style: .rigid)` | rigid impact (collision, firm) | iOS 13+ |
| `UINotificationFeedbackGenerator().notificationOccurred(.success)` | success semantic | iOS 10+ |
| `UINotificationFeedbackGenerator().notificationOccurred(.warning)` | warning semantic | iOS 10+ |
| `UINotificationFeedbackGenerator().notificationOccurred(.error)` | error semantic | iOS 10+ |
| `UISelectionFeedbackGenerator().selectionChanged()` | item-pick / value-change | iOS 10+ |
| `UICanvasFeedbackGenerator().alignmentOccurred(at:)` | alignment (Apple Pencil) | iOS 17.5+ (NOT visionOS / tvOS / watchOS) |
| `UICanvasFeedbackGenerator().pathCompleted(at:)` | path completion (Apple Pencil) | iOS 17.5+ (NOT visionOS / tvOS / watchOS) |

### SwiftUI (`SensoryFeedback`)

| Pattern | Documented semantic | Available |
|---|---|---|
| `SensoryFeedback.success` | success semantic | iOS 17+ / macOS 14+ / ... (per-pattern playback varies) |
| `SensoryFeedback.warning` | warning semantic | iOS 17+ / macOS 14+ / ... (per-pattern playback varies) |
| `SensoryFeedback.error` | error semantic | iOS 17+ / macOS 14+ / ... (per-pattern playback varies) |
| `SensoryFeedback.selection` | item-pick / value-change | iOS 17+ / macOS 14+ / ... (per-pattern playback varies) |
| `SensoryFeedback.impact(weight: .light)` | light impact | iOS 17+ / macOS 14+ / ... (per-pattern playback varies) |
| `SensoryFeedback.impact(weight: .medium)` | medium impact | iOS 17+ / macOS 14+ / ... |
| `SensoryFeedback.impact(weight: .heavy)` | heavy impact | iOS 17+ / macOS 14+ / ... |
| `SensoryFeedback.alignment` | alignment | iOS 17.5+ (per-pattern playback varies) |
| `SensoryFeedback.pathComplete` | path completion | iOS 17.5+ (per-pattern playback varies) |
| `SensoryFeedback.increase` | increase semantic | iOS 17+ |
| `SensoryFeedback.decrease` | decrease semantic | iOS 17+ |
| `SensoryFeedback.start` | start semantic | iOS 17+ |
| `SensoryFeedback.stop` | stop semantic | iOS 17+ |

The Skill records API existence + base availability. **Per-pattern playback availability requires per-pattern DocC verification** — the Skill does NOT claim that every pattern plays identically on every platform.

### AppKit (`NSHapticFeedbackManager` / `NSHapticFeedbackPerformer`)

| Pattern | Documented semantic | Available |
|---|---|---|
| `NSHapticFeedbackPattern.generic` | generic feedback (default) | macOS 10.10+ |
| `NSHapticFeedbackPattern.alignment` | alignment (guides, best fit) | macOS 10.10+ |
| `NSHapticFeedbackPattern.levelChange` | discrete pressure zone changes | macOS 10.10+ |

AppKit haptics are trackpad-dependent. The Skill notes: native Mac haptics are NOT iPhone-style; Mac Force Touch / trackpad hardware determines playback.

### WatchOS (`WKInterfaceDevice.current().play(_:)`)

`WKInterfaceDevice.current().play(_:)` (watchOS 2.0+) plays haptic feedback on Apple Watch. Per-pattern playback depends on hardware.

### Core Haptics (ADVANCED / CONDITIONAL)

`CHHapticEngine` / `CHHapticPattern` / `CHHapticEvent` (iOS 13+) provide lower-level custom-pattern APIs. Core Haptics is labeled ADVANCED / CONDITIONAL by the Skill:

- Used only when system patterns are insufficient.
- Used only when the custom tactile experience has semantic / product value.
- NOT used for "premium feel" alone.

The Skill explicitly rejects the fabricated `CHHapticPatternComposer` (does not exist in installed SDK).

## Causal relationship

The Skill publishes: haptic feedback should reinforce a meaningful cause/effect. The user must understand:

- What triggered the haptic.
- What the haptic means.

Examples:

- A button press → a click haptic. Causal. Reinforced.
- A delete action → a warning haptic BEFORE the delete, then a success haptic AFTER. Causal. Reinforced.
- A typo in the user's name → a warning haptic at the form field. Causal. Reinforced.
- Every single tap → a haptic. NOT causal — degrades meaning.

## Consistency

Within a single user flow, the Skill publishes:

- Use the same haptic for the same semantic event.
- Don't switch between light / medium / heavy impact for the same kind of action.
- Match UIKit / SwiftUI patterns where both are used (a SwiftUI app wrapping UIKit components should still use the SwiftUI `SensoryFeedback` for consistency, or vice versa, depending on the dominant framework).

## Overuse prevention

Hard principle: feedback should reinforce meaningful cause/effect, not become background noise.

The Skill rejects:

- **Haptic on every tap** — degrades meaning; user stops noticing.
- **Haptic on every selection change** — fine for explicit pickers (selectable list), NOT for implicit state changes (e.g. slider value updating while user drags).
- **Haptic as decoration** — using haptic because it feels "nice" without semantic justification.

## Capability checking

The Skill recommends capability checking before playing haptic:

- iOS: most modern iPhones have a Taptic Engine; older devices and iPads may have limited / no haptic capability.
- macOS: haptics require Force Touch / trackpad hardware; not all Macs have it.
- visionOS: spatial feedback system; not the same as UIKit haptics.
- watchOS: every Apple Watch has haptic capability, but the user may disable it.

The Skill recommends: feature-detect and gracefully skip when capability is missing.

## Accessibility

- **User preferences**: respect system settings (Reduce Motion affects haptic timing; user may have haptic feedback disabled in Accessibility settings).
- **Reduced motion**: Apple's Reduce Motion accessibility setting may reduce or disable certain haptics; the Skill recommends honoring this.
- **Equal access**: critical information must not depend on haptics alone. Visual + text + audio fallback.

## What the Skill rejects

- **Haptic Everywhere** — playing on every action. Rejected.
- **Haptic Numerology** — invented canonical numeric intensity scale (e.g. "error = intensity 1.0"). Rejected.
- **Semantic Haptic Swap** — using error pattern for success because it "feels stronger". Rejected.
- **Haptic Without Cause** — playing feedback that doesn't correspond to a user action or system event. Rejected.
- **Custom Haptics by Default** — using Core Haptics merely for "premium feel". Rejected.
- **Platform Feedback Parity** — assuming iPhone-style haptics everywhere. Rejected.
- **Haptic-Only Meaning** — relying on haptics alone for critical information. Rejected.

## What the Skill does NOT do

- Does NOT invent canonical numeric feedback intensity scales.
- Does NOT cite `CHHapticPatternComposer`, `UIWindowSceneFeedback`, or `NSHapticEngine` (fabricated).
- Does NOT claim identical playback of every pattern across platforms.
- Does NOT mandate frequency / overuse thresholds as canonical Apple rules.