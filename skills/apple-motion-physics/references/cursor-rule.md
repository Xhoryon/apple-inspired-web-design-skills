# Cursor Rule Reference

The Web cursor must not be replaced. The pointer highlight surface moves; the native cursor remains. This file is the Tier 2 reference for `apple-motion-physics` and `apple-pointer-interaction`.

> **HARD RULE:** On Web, the native cursor must NOT be replaced or hidden by a custom cursor. The pointer highlight is a separate surface that follows the pointer near the target; the cursor itself is preserved.

## Why the rule exists

A Web application that hides the native cursor and shows a custom one is fighting the platform. The user expects the native cursor; the platform provides it; the application should work with it.

Replacing the cursor:

- Breaks the user's expectation of where the cursor is.
- Hides platform-provided affordances (text selection, drag handles, accessibility indicators).
- Interferes with accessibility tools (the cursor is the input indicator for many assistive technologies).
- Conflicts with the system cursor on hybrid devices (the user may switch between touchpad and touchscreen).

The Skill rejects cursor replacement on Web. The pointer highlight is a separate surface that follows the pointer near the target; the cursor remains.

## What the pointer highlight surface does

The pointer highlight surface is a DOM element (a `<div>`, a `::before`, a `::after`, or a dedicated layer) that:

- Appears when the pointer is over a target.
- Follows the pointer near the target (with a 1:1 retargeting rule on iPadOS 26+; APPROXIMATE on Web).
- Disappears when the pointer leaves the target.
- Retargets from its current state when the pointer moves between targets.

The highlight surface is NOT the cursor. The cursor is the platform's. The highlight is a separate visual indicator of the target.

## What the Skill rejects

- A Web app that hides the native cursor with `cursor: none`.
- A Web app that replaces the native cursor with a custom DOM element.
- A Web app that adds cursor magnetism (the cursor "snaps" to the nearest target). The Skill records this as ABANDON — see the source matrix and platform matrix.
- A Web app that uses the cursor as the moving surface (the cursor moves and the highlight stays).

## Reduced motion

The pointer highlight surface respects `prefers-reduced-motion: reduce`. The surface still moves between targets (the state change is preserved), but the movement is shorter and faster. The Skill must NOT remove the retargeting entirely on reduced motion.

## What the surface is not

The pointer highlight surface is NOT:

- A custom cursor.
- A tooltip.
- A focus indicator.
- A selected indicator.

The pointer highlight is a transient visual indicator of the target. It is distinct from the focus indicator (persistent, marks the focused element) and the selected indicator (persistent, marks the user's choice).

## Cross-Skill note

- The pointer-target relationship is `apple-pointer-interaction` (this Skill).
- The retargeting rule is `retargeting.md`.
- The state semantics are `apple-control-states`.
- The platform-applicability matrix is `apple-experience-foundation/APPLE-EXPERIENCE-PLATFORM-MATRIX.md`.
