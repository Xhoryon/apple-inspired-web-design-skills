# Retargeting Reference

Retargeting is a specific case of interruption: the visual surface in motion takes a new target, and the in-flight motion continues toward the new target from its current visual state. This file is the Tier 2 reference for `apple-pointer-interaction` and `apple-motion-physics`.

## The rule

> When user input changes the target mid-motion, the visual surface in motion continues toward the new target from its current visual state. The motion does NOT "finish A then start B."

This is the retargeting rule. It is a HARD INVARIANT.

## Why the rule exists

If a transition must finish its current animation before starting a new one:

- The user sees a visible pause when changing targets quickly.
- The visual surface feels "stuck" between targets.
- The interaction feels laggy and unresponsive.

A retargeted transition:

- Continues smoothly between targets.
- The visual surface reaches the new target as quickly as possible.
- The interaction feels responsive and natural.

Apple's iPadOS 26+ pointer highlight uses retargeting. The highlight surface continues from its current visual state when the pointer moves between targets.

## The "current state" concept

The "current state" of an in-flight motion is:

- Current position.
- Current size.
- Current visual state (color, opacity, etc.).
- Current velocity.

A retargeted transition uses all of these as the starting point for the new transition. The new transition is not "from the default state" — it is "from the current state to the new target."

## Code shape

A common implementation pattern (Web, CSS transition with custom properties):

```css
.highlight {
  transition: transform 240ms var(--ease), width 240ms var(--ease);
  transform: translateX(var(--current-x, 0));
  width: var(--current-w, 0);
}

.cluster .target:hover ~ .highlight,
.cluster .target:focus-visible ~ .highlight {
  /* Set --current-x and --current-w to the target's geometry. */
  /* The transition handles the rest. */
}
```

Or with WAAPI:

```javascript
highlight.animate([
  { transform: `translateX(${currentX}px)`, width: `${currentW}px` },
  { transform: `translateX(${targetX}px)`, width: `${targetW}px` }
], { duration: 240, fill: 'forwards', easing: 'cubic-bezier(...)' });
```

The animate call always uses the current visual state as the starting point of the new animation.

## Common mistakes

- **Queued animations** — finish A, then start B. Wrong.
- **Reset to default** — always animate from a default state. Wrong.
- **Snap to new target instantly** — no animation. Loses visual continuity. Wrong (unless the user has `prefers-reduced-motion: reduce`).
- **Interpolate from a wrong starting point** — the implementation accidentally animates from the default state or from a stale cached state. Wrong.

## Pointer exit (mid-flight)

If the pointer exits all targets mid-flight, the highlight's exit behavior is determined by the cluster's exit behavior (see `shared-highlight.md`):

- Dissolve (fade out).
- Return to the selected target.
- Return to a "home" position.

The exit transition should also retarget from the current state. The highlight is currently at A; it animates from A's geometry to the exit state (transparent, or selected target, or home).

## Native iPadOS 26+

The Apple system primitive retargets from the current state. The Skill recommends the system primitive, not a custom re-implementation.

## Reduced motion

`prefers-reduced-motion: reduce` reduces the amplitude of the retargeting. The highlight still moves between targets (state change is preserved), but the movement is shorter and faster. The Skill must NOT remove the retargeting entirely on reduced motion.

## Cross-Skill note

The retargeting rule is shared between `apple-pointer-interaction` (where the rule originated) and `apple-motion-physics` (where the physics of the retargeting is described). Both Skills cite this file.
