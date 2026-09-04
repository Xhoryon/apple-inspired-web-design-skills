# Cursor Rule Reference (Pointer-Interaction View)

The Web cursor must not be replaced. The pointer highlight surface moves; the native cursor remains. This file is the Tier 2 reference for `apple-pointer-interaction`.

The same principle is recorded from the motion-physics angle in `apple-motion-physics/references/cursor-rule.md`; this file is the pointer-target angle. They describe the same rule; both Skills cite the principle.

> **HARD RULE:** On Web, the native cursor must NOT be replaced or hidden by a custom cursor. The pointer highlight is a separate surface that follows the pointer near the target; the cursor itself is preserved.

## Why the rule exists

A Web application that hides the native cursor and shows a custom one is fighting the platform. The user expects the native cursor; the platform provides it; the application should work with it.

Replacing the cursor:

- Breaks the user's expectation of where the cursor is.
- Hides platform-provided affordances (text selection, drag handles, accessibility indicators).
- Interferes with accessibility tools (the cursor is the input indicator for many assistive technologies).
- Conflicts with the system cursor on hybrid devices.

The Skill rejects cursor replacement on Web. The pointer highlight is a separate surface; the cursor remains.

## What the pointer highlight surface is

The pointer highlight surface is a DOM element that:

- Appears when the pointer is over a target.
- Follows the pointer near the target.
- Disappears when the pointer leaves the target.
- Retargets from its current state when the pointer moves between targets.

The highlight surface is NOT the cursor. The cursor is the platform's. The highlight is a separate visual indicator.

## Pointer magnetism

Pointer magnetism (the cursor snapping to the nearest target) is ABANDON on Web. The Skill records this in the platform-applicability matrix. A Web implementation that adds cursor snap is wrong; it fights the platform.

## Reduced motion

The pointer highlight surface respects `prefers-reduced-motion: reduce`. The surface still moves between targets (the state change is preserved), but the movement is shorter and faster.

## Cross-Skill note

- The motion-physics view of the same rule is `apple-motion-physics/references/cursor-rule.md`.
- The retargeting rule is `retargeting.md`.
- The state semantics are `apple-control-states`.
- The shared highlight (a single highlight across a cluster) is `shared-highlight.md`.
- The platform-applicability matrix is `apple-experience-foundation/APPLE-EXPERIENCE-PLATFORM-MATRIX.md`.
