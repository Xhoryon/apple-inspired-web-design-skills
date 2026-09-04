# Pointer Version Notes (Pointer-Interaction Reference)

Pointer behavior is version-sensitive. iPadOS 26 introduced a more direct 1:1 pointer highlight with retargeting; older iPadOS had different semantics. This file is the Tier 2 reference for `apple-pointer-interaction`.

The canonical version notes live in the foundation root: see [`../../../pointer-version-notes.md`](../../../pointer-version-notes.md) (relative to this file). This file summarizes the current vs. legacy distinction so the Skill can tag version-sensitive behavior without duplicating the full notes.

## Current vs. legacy pointer behavior

| Property | iPadOS 26+ (current) | iPadOS ≤ 25 (legacy) |
|---|---|---|
| Pointer highlight | Direct 1:1 follow; separate highlight surface that retargets | Highlight, lift, hover, magnetism semantics |
| Highlight retargeting | Retargets from current state (no "finish A then start B") | Different retargeting behavior |
| Cursor | The cursor remains; the highlight surface moves | The cursor may be replaced or magnetized |
| Native primitive | `UIHoverEffect` / system | Older UIKit semantics |
| Where documented | WWDC25 "Elevate the design of your iPad app" + iPadOS 26 release notes | HIG Pointing devices (older) |

The Skill MUST tag any reference to iPadOS 26+ behavior as VERSION-SPECIFIC. The current vs. legacy distinction is not merged.

## Why this matters

A Skill that recommends the iPadOS 26+ pointer behavior on iPadOS ≤ 25 is wrong. The visual effect would not be supported, and the legacy semantics would conflict. The Skill requires:

- On iPadOS 26+: use the system primitive (NATIVE).
- On iPadOS ≤ 25: use the legacy semantics (NATIVE for the older generation).
- On Web: APPROXIMATE (not system behavior).
- On Touch Web: ABANDON (no pointer; use press + selection + touch feedback).

## What the Skill rejects

- A Skill that recommends the iPadOS 26+ behavior without tagging it VERSION-SPECIFIC.
- A Skill that claims the iPadOS 26+ behavior is the timeless Apple rule.
- A Skill that merges current and legacy into a single recommendation.
- A Web implementation that imitates the iPadOS 26+ pointer behavior as if it were system behavior (it is APPROXIMATE; tag it as such).

## Web approximation

The Web approximation of the iPadOS 26+ pointer highlight is:

- A highlight surface (a DOM element) that follows the pointer.
- The surface retargets from its current state (not "finish A then start B").
- The native cursor is NOT replaced.
- The highlight surface is gated by `(hover: hover) and (pointer: fine)`.

The Web approximation is APPROXIMATE, not system behavior. The Skill tags it as such.

## Full version notes

The full version notes (with timeline, source citations, and detailed semantic differences) are in the foundation root at `pointer-version-notes.md`. This file is a Skill-level summary.

## Cross-Skill note

- The pointer-target relationship is `apple-pointer-interaction` (this Skill).
- The retargeting rule is `retargeting.md`.
- The pointer modality gating is `pointer-modality.md`.
- The platform-applicability matrix is `apple-experience-foundation/APPLE-EXPERIENCE-PLATFORM-MATRIX.md`.
- The source matrix is `apple-experience-foundation/APPLE-EXPERIENCE-SOURCE-MATRIX.md`.
