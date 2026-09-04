# Shared Highlight Reference

A shared highlight is a single visual surface that animates between targets in a cluster. This file is the Tier 2 reference for `apple-pointer-interaction`.

## When to use

Use a shared highlight for:

- Segmented control
- Tab bar
- Toolbar
- Compact navigation cluster
- Filter chip cluster
- Toggle group

Do NOT use a shared highlight for:

- Large card grids
- Data table rows
- Whole-page sections
- Distant, unrelated controls

A shared highlight implies that the targets are related and form a group. A button on a far-away toolbar should not share a highlight with a button on a sidebar.

## State model

A shared highlight has three logical states:

- **Current target** — the target the pointer is currently on (or the last target the pointer was on before exit).
- **Previous target** — the target the highlight was on before the current one. Used for retargeting direction.
- **No target** — the pointer is not on any cluster target. The highlight dissolves or remains in a "home" position.

A shared highlight also has:

- **Position** — current visual position.
- **Width / height** — current visual size.
- **Velocity** — current motion (for retargeting from current state).

## Retargeting from current state

When the pointer moves from target A to target B, the shared highlight animates from its current visual state to B's geometry. The current state is the starting point.

```text
current: at A, velocity 0
pointer moves to B
shared highlight: from at A, velocity 0 → at B
```

NOT:

```text
current: at A, velocity 0
pointer moves to B
shared highlight: at A → at A → at B (queued, wrong)
shared highlight: at default → at B (restarted, wrong)
```

A Skill that queues the shared highlight's animation is wrong. A Skill that resets the highlight to a default state is wrong. The correct behavior is retargeting from the current state.

## Pointer exit

When the pointer leaves all cluster targets, the shared highlight:

- Dissolves (fades out) — common in toolbars.
- Returns to the previously selected target — common in segmented controls.
- Returns to a "home" position — common in tab bars.

The exit behavior depends on the cluster's semantic. A Skill that picks the wrong exit behavior for a given cluster is wrong.

## Selection vs hover vs focus

The shared highlight typically visualizes the **hover** state, not the **selected** state and not the **focus** state. The selected state and the focus state have their own visual treatments, distinct from the shared hover highlight.

A Skill that uses the shared hover highlight as the selected indicator is wrong. A Skill that uses the shared hover highlight as the focus indicator is wrong.

## Reduced motion

`prefers-reduced-motion: reduce` reduces the shared highlight's amplitude. The highlight still moves between targets (state change is preserved), but the movement is shorter and faster.

A Skill that removes the shared highlight entirely on reduced motion is wrong — the user must still be able to tell which target is active. A reduced highlight that is still visible is correct.

## Native iPadOS 26+

iPadOS 26+ provides a system-level shared highlight for controls that opt in (`UIHoverEffect`, `UIPointerInteraction`). The Skill recommends the system primitive, not a custom re-implementation.

## Native macOS

`NSPopoverTouchBarItem` and related macOS APIs provide shared highlight on the Touch Bar. The Skill does not implement a custom shared highlight for macOS unless the project requires a custom visual.

## Desktop Web (APPROXIMATE)

The Web implementation is an approximation. Common patterns:

- A single `<div class="shared-highlight">` element absolutely positioned within the cluster container.
- On pointer enter on a target, the highlight's `transform` and `width` / `height` animate to the target's bounding box.
- On pointer leave, the highlight's opacity animates to 0 (dissolve) or back to the selected target.

The implementation must NOT replace the native cursor. The highlight surface moves; the cursor does not.

## Touch Web (ADAPT)

On touch-only Web, the shared highlight is ADAPTED: instead of a transient hover highlight, the control shows a persistent focus / selected indicator. The pointer concept does not apply; the intent is preserved by making the selected state clearly visible.

## Native iOS (ADAPT)

iOS does not have a shared hover highlight (touch has no hover). The selected indicator on iOS is the persistent selection state. The shared highlight concept does not apply.

## Implementation patterns

A common Web pattern with CSS:

```css
.cluster {
  position: relative;
}

.cluster .target {
  position: relative;
  z-index: 1;
}

.cluster .highlight {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: var(--target-width);
  transform: translateX(var(--target-offset));
  transition: transform 240ms var(--ease-spring), width 240ms var(--ease-spring);
  background: var(--accent-tint);
  border-radius: inherit;
  pointer-events: none;
  z-index: 0;
}
```

On pointer enter on a target, the highlight's `--target-width` and `--target-offset` custom properties are updated to the target's geometry. The transition animates from the previous geometry to the new geometry — this is retargeting from current state.

## Cross-Skill note

The shared highlight is a reference inside `apple-pointer-interaction` (Tier 2). The state semantics (`hover` / `selected` / `focused`) are in `apple-control-states`. The motion physics of the shared highlight's animation is in `apple-motion-physics` (retargeting from current state). The platform-applicability verdict is in `APPLE-EXPERIENCE-PLATFORM-MATRIX.md`.
