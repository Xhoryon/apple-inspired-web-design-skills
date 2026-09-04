# Retargeting Physics Reference

The motion-physics view of retargeting. This file is the Tier 2 reference for `apple-motion-physics`. The rule is `apple-pointer-interaction/references/retargeting.md`; this file describes the physics of the retargeting motion.

> **HARD RULE:** When user input changes the target mid-motion, the visual surface in motion continues toward the new target from its current visual state. The motion does NOT "finish A then start B."

## The physics

The retargeting motion is a spring (or other physics-based motion) that:

- Starts at the current visual state (current position, current size, current visual treatment, current velocity).
- Ends at the new target's geometry.
- Uses the same spring stiffness / damping as the original motion (no jump in physics).
- Maintains continuity (no reset of velocity to zero; the motion continues smoothly).

The retargeting motion is the same physics as a fresh motion, except the starting point is the current state, not the default state.

## Spring model for retargeting

The spring equation is the same as for any spring motion (see `spring-response.md`):

```
F = -k * x - c * v
```

where:

- `F` is the force.
- `k` is the spring stiffness.
- `x` is the displacement from the target.
- `c` is the damping coefficient.
- `v` is the velocity.

The retargeting motion sets the initial state to the current visual state:

- `x_0` = current displacement from the new target.
- `v_0` = current velocity.

The spring then settles on the new target.

## Common mistakes

- **Reset to default** — always animate from a default state. The retargeting rule rejects this.
- **Queued animations** — finish A, then start B. The retargeting rule rejects this.
- **Snap to new target instantly** — no animation. Loses visual continuity.
- **Interpolate from a wrong starting point** — the implementation accidentally animates from the default state or from a stale cached state.

## Code shape (Web, WAAPI)

```javascript
highlight.animate([
  { transform: `translateX(${currentX}px)`, width: `${currentW}px` },
  { transform: `translateX(${targetX}px)`, width: `${targetW}px` }
], { duration: 240, fill: 'forwards', easing: 'cubic-bezier(...)' });
```

The animate call always uses the current visual state as the starting point of the new animation. The duration and easing are HEURISTICS; the implementer tunes them.

## Code shape (Web, CSS transition)

```css
.highlight {
  transition: transform 240ms var(--ease), width 240ms var(--ease);
  transform: translateX(var(--current-x, 0));
  width: var(--current-w, 0);
}

.cluster .target:hover ~ .highlight,
.cluster .target:focus-visible ~ .highlight {
  /* Set --current-x and --current-w to the target's geometry. */
}
```

The CSS transition uses custom properties; the transition handles the rest. The starting point is the current state of the custom properties.

## Pointer exit (mid-flight)

If the pointer exits all targets mid-flight, the highlight's exit behavior is determined by the cluster's exit behavior:

- Dissolve (fade out).
- Return to the selected target.
- Return to a "home" position.

The exit transition should also retarget from the current state. The highlight is currently at A; it animates from A's geometry to the exit state (transparent, or selected target, or home).

## Native iPadOS 26+

The Apple system primitive retargets from the current state. The Skill recommends the system primitive, not a custom re-implementation.

## Reduced motion

`prefers-reduced-motion: reduce` reduces the amplitude of the retargeting. The highlight still moves between targets (state change is preserved), but the movement is shorter and faster. The Skill must NOT remove the retargeting entirely on reduced motion.

## Cross-Skill note

- The retargeting rule (pointer-target view) is `apple-pointer-interaction/references/retargeting.md`.
- The spring response (full physics) is `spring-response.md`.
- The reduced-motion rule is `reduced-motion.md`.
- The input-sensitive amplitude is `input-sensitive-amplitude.md`.
- The pointer highlight must not affect the native cursor is `cursor-rule.md`.
