# Progress & Status

The Skill's progress and status model. Determinate vs indeterminate, honest representation, stall handling, completion, cancellation.

## Two modes

| Mode | When to use | Apple surface |
|---|---|---|
| **Determinate** | when duration / progress is meaningfully measurable | `ProgressView(value:total:)` (SwiftUI); `UIProgressView.progress` (UIKit, legacy); `NSProgressIndicator` (AppKit) |
| **Indeterminate** | when the operation's progress is NOT meaningfully measurable | `ProgressView()` (SwiftUI, no value binding); `UIProgressView` with `.indeterminate` style; `NSProgressIndicator.indeterminate` / `.spinning` |

## Hard invariants

- **Never fake precision.** No invented progress percentages. No fake time remaining. The skill forbids `0 → 90% instantly, then sit at 90% for five minutes` patterns.
- **Adapt as the operation becomes measurable.** If an operation begins indeterminate and becomes measurable, switch to determinate with the actual progress.
- **Surface failure rather than leave a frozen spinner.** If the operation stalls, transitions to error, or completes outside the spinner surface, the skill surfaces that state.
- **Cancellation where feasible.** A long-running operation should be cancellable when the platform supports it. iOS: swipe-to-cancel; macOS: progress UI may include a Cancel button; Web: same.

## Truthful representation

The skill rejects misleading progress representation:

- **Jumping ahead**: presenting 90% complete when 0% has actually completed is misleading. Reject.
- **Stalling at a value**: if the operation stalls, surface the stall rather than continuing to display a stale percentage.
- **Unknown → known transition**: when an operation begins without a known duration and later becomes measurable, switch to determinate.
- **Completion state**: when the operation completes, transition to a completion indicator (check mark / "Done" / outcome feedback owned by `apple-feedback-response`).

## Apple platform surfaces

| Platform | Determinate | Indeterminate | Notes |
|---|---|---|---|
| iOS / iPadOS | `ProgressView(value:total:)` (iOS 14+) | `ProgressView()` (iOS 14+) | SwiftUI preferred; `UIProgressView` is legacy |
| macOS | `ProgressView(value:total:)` (macOS 11+) | `ProgressView()` (macOS 11+) | `NSProgressIndicator` (AppKit) for native macOS |
| watchOS | `ProgressView(value:total:)` (watchOS 7+) | `ProgressView()` (watchOS 7+) | Apple HIG watchOS guidance is platform-specific; verify per-version |
| visionOS | `ProgressView(value:total:)` (visionOS 1.0+) | `ProgressView()` (visionOS 1.0+) | SwiftUI preferred |
| Web | `<progress value="0.5" max="1.0">` element + ARIA `progressbar` role | `<progress>` (no value) + ARIA `progressbar` role | Browser-native; `aria-live="polite"` for status updates |

## Transition matrix

| From | To | When |
|---|---|---|
| Indeterminate | Determinate | operation duration becomes measurable |
| Determinate | Indeterminate | operation duration becomes unmeasurable (rare; usually indicates failure) |
| Either | Error / Failure | operation fails |
| Either | Completion | operation completes successfully |
| Either | Cancelled | user cancels (where supported) |
| Determinate | Frozen / Stuck | operation stalls (timeout / no progress for N seconds) |

## What the skill rejects

- **0 → 90% instantly** to make the operation "feel faster". Reject.
- **Frozen spinner forever** when the operation has failed. Reject; surface failure.
- **Progress UI that disappears before completion** because of structural reflow (e.g. sidebar collapse). The progress information must survive structural change; coordinate with `apple-adaptive-structure`.
- **Fake time remaining** like "5 seconds remaining" when the operation has no known duration. Reject.
- **Progress percentage that contradicts actual work** (e.g. claiming 50% complete when only 10% of bytes have transferred). Reject.

## Coordination with other Skills

- **`apple-control-states`**: progress UI itself is a control. Its state (idle / running / paused / completed / failed) is owned by `apple-control-states`.
- **`apple-motion-physics`**: progress UI animation (fill animation, completion check mark draw) is owned by `apple-motion-physics`.
- **`apple-modality-overlays`**: a progress sheet is a sheet (modality); feedback decides whether a sheet is the right answer; modality decides the sheet presentation.
- **`apple-adaptive-structure`**: progress UI must survive structural reflow; the Skill publishes the persistence rule.
- **`apple-feedback-response`**: progress UI is one of the seven feedback intents; the Skill owns the **meaning** of "this operation is in progress", not the animation or the structural placement.