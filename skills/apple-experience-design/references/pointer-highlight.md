# Pointer Highlight Reference (Web Approximation)

The pointer highlight on Web approximates the iPadOS 26+ target highlight. This file is the Tier 2 reference for `apple-experience-design` and `apple-pointer-interaction`. The pointer-target relationship is `apple-pointer-interaction`; the motion physics is `apple-motion-physics`.

> **EVIDENCE-CLASS STATEMENT:** Web pointer highlight is APPROXIMATE on Web. It is not Apple system behavior. The Skill tags it as such.

## What the pointer highlight is

A pointer highlight is a transient visual surface that:

- Appears when the pointer is over a target.
- Follows the pointer near the target.
- Disappears when the pointer leaves the target.
- Retargets from its current state when the pointer moves between targets.

The pointer highlight is distinct from:

- The native cursor (the cursor is the platform's; the highlight is separate).
- The focus indicator (persistent; marks the focused element).
- The selected indicator (persistent; marks the user's choice).

## Native iPadOS 26+

The Apple system primitive provides the pointer highlight on iPadOS 26+:

- A direct 1:1 highlight that follows the pointer.
- A retargeting highlight surface.
- The cursor remains; the highlight surface moves.

The Skill recommends the system primitive. A custom re-implementation is rarely justified on iPadOS 26+.

## Web approximation

The Web approximation:

- A DOM element (a `<div>`, a `::before`, a `::after`, or a dedicated layer) that follows the pointer.
- The surface retargets from its current state (see `apple-motion-physics/references/retargeting.md`).
- The native cursor is NOT replaced.
- The highlight surface is gated by `(hover: hover) and (pointer: fine)` (see `apple-pointer-interaction/references/pointer-modality.md`).

The Web approximation is APPROXIMATE, not system behavior. The Skill tags it as such.

## Touch-only Web

On touch-only Web, the pointer highlight is ABANDON. Touch has no fine pointer; the highlight would be a tap-and-stay artifact. The Skill recommends press + selection + touch feedback for touch-only devices.

## Reduced motion

The pointer highlight surface respects `prefers-reduced-motion: reduce`. The surface still moves between targets (the state change is preserved), but the movement is shorter and faster.

## Cross-Skill note

- The pointer-target relationship is `apple-pointer-interaction` (this Skill).
- The retargeting rule is `apple-motion-physics/references/retargeting.md`.
- The pointer modality gating is `apple-pointer-interaction/references/pointer-modality.md`.
- The platform-applicability matrix is `apple-experience-foundation/APPLE-EXPERIENCE-PLATFORM-MATRIX.md`.
- The source matrix is `apple-experience-foundation/APPLE-EXPERIENCE-SOURCE-MATRIX.md`.
