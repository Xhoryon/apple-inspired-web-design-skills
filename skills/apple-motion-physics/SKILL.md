---
name: apple-motion-physics
description: Use when designing or implementing the physics of an interaction transition — spring response, damping, interruption, retargeting from current state, input-sensitive amplitude, reduced-motion adaptation. Applies to control-state transitions, pointer highlight retargeting, and shared highlight animation. The Skill treats motion values as perceptual heuristics, not Apple canonical numbers. Do NOT use for the state model itself (use apple-control-states), the pointer-target relationship (use apple-pointer-interaction), page-level composition, or general Web spatial motion. Do NOT write fake Apple canonical numeric values.
version: "0.1.0-dev"
license: MIT
---

# apple-motion-physics

How an interaction transition moves. Spring response, damping, interruption, retargeting from current state, input-sensitive amplitude.

## When to use

Use when the task involves the physics of a state change. Symptoms:

- "The press animation feels mechanical. Use spring instead of linear."
- "The shared highlight retargets from its current visual state, not from a default."
- "When the user moves the pointer to a new target mid-animation, the highlight follows the new target, not finishes the old one."
- "The press feedback amplitude is different on touch vs. mouse / trackpad."
- "When the user has `prefers-reduced-motion: reduce`, the press animation is still there but smaller and faster."

Do **not** use when:

- The task is the state model. Use `apple-control-states`.
- The task is the pointer-target relationship. Use `apple-pointer-interaction`.
- The task is general Web spatial motion (page transitions, scroll choreography). Use the stable `apple-motion-interaction`.
- The task is to write fake Apple canonical numeric values. The Skill is explicit: numeric values are HEURISTICS.

## Hard invariants

1. **No fake Apple canonical numeric values.** The Apple HIG does not publish canonical spring constants for third-party implementations. A Skill that writes "Apple spring = stiffness 320, damping 30" is unsupported and misleading. The Skill teaches perceptual heuristics (response feel, settling, overshoot) and the platform tools that approximate them.
2. **Interruptibility.** When user input can redirect a transition mid-flight, the motion should follow the new target, not finish the old. This is the retargeting rule.
3. **Input-sensitive amplitude.** Touch and mouse / trackpad have different surface areas. Touch press feedback may be larger; mouse / trackpad may be more restrained. The Skill recommends amplitude ranges, not exact values.
4. **Reduced-motion preserves state.** When `prefers-reduced-motion: reduce` is set, large motion is reduced or removed. State transitions still happen (color, opacity, small scale). The user must still be able to tell that something happened.
5. **Framework-neutral.** The Skill is not a Framer Motion tutorial, not a GSAP tutorial, not a CSS tutorial. It teaches principles. The implementation layer (WAAPI, CSS, JS spring, native) is the implementer's choice.

## Motion principles

### Spring response

A spring with critical damping reaches the target without overshooting. A spring with under-damping overshoots. A spring with over-damping approaches slowly. The Skill recommends:

- Press feedback: light to medium damping, fast response. The user expects the visual change to be immediate.
- Release / recovery: light to medium damping, slightly slower than press. The user expects the visual change to settle.
- Highlight retargeting (iPadOS 26-style): critical damping to medium under-damping. The user expects the highlight to "settle" on the new target.
- Page transitions: not in scope of this Skill.

These are HEURISTICS. The implementer tunes them.

### Damping concept

Damping is the resistance to motion. A spring with critical damping reaches its target without oscillating. Under-damping overshoots. Over-damping approaches slowly.

For press feedback: under-damping is usually appropriate (small overshoot feels "alive"). For release: critical or slightly under-damping. For highlight retargeting: critical or near-critical damping.

### Velocity

Velocity is the rate of change of position. A transition has an initial velocity (often zero) and a final velocity (often zero). Interruptibility means the new target takes the current velocity into account, so the transition feels continuous.

### Interruption

If a transition is mid-flight and the user input changes the target, the transition should:

- Update to the new target.
- Use the current visual state as the starting state.
- Continue with the same motion timing.

A transition that finishes its current animation and starts a new one is "queued" and is wrong. A transition that resets to a default state is "restarted" and is wrong. The correct behavior is "retargeted" or "interrupted".

### Retargeting

Retargeting is a specific case of interruption: the visual surface in motion (e.g. a shared highlight) takes a new target, and the in-flight motion continues toward the new target from its current visual state. The motion does not "finish A then start B." It is "A in flight, take B as the new destination, continue."

## Input-sensitive amplitude

The same conceptual transition may have different amplitude on different input modalities:

- **Touch** — the entire control surface is the touch area. A press feedback amplitude of 0.04 (scale) or 8% (opacity) is appropriate.
- **Mouse / trackpad** — the pointer is a small target. A press feedback amplitude of 0.02 (scale) or 4% (opacity) is appropriate.
- **Keyboard** — there is no pointer trajectory. The press feedback is a discrete state change, not a motion. Motion should communicate state without depending on pointer.

These are HEURISTICS. The implementer tunes them.

## Reduced-motion adaptation

`prefers-reduced-motion: reduce` must reduce or remove large motion. Concretely:

- Reduce duration: from 280 ms to ≤ 80 ms.
- Reduce amplitude: from 0.04 scale to ≤ 0.01 scale.
- Remove translation: from 8 px to 0 px.
- Keep state transitions: opacity (instant or very fast), color (instant or very fast), border (instant or very fast).

The user must still be able to tell that the state changed. Reduced motion is not "no motion"; it is "less motion while preserving essential feedback."

## Web implementation patterns

On Web, the Skill recommends:

- CSS `transition` with `transform` and `opacity` (compositor-friendly).
- `prefers-reduced-motion: reduce` media query gates the transition.
- WAAPI (`Element.animate()`) for programmatic, interruptible animations.
- JavaScript spring libraries (Framer Motion, Motion One, etc.) when more sophisticated physics is needed.
- CSS `transform: translate3d(0,0,0)` to force compositor layer for transform transitions.

The Skill does NOT recommend:

- Continuous `requestAnimationFrame` loops for state transitions (use CSS or WAAPI).
- Animating non-compositor properties (`width`, `height`, `top`, `left`).
- Full-page filter surfaces on every interaction.
- Animations that block input handling for more than 16 ms.

These are HEURISTICS, not absolute rules. The implementer judges based on the device and use case.

## Cross-Cutting Contract Hook

Visual Authenticity Contract (`APPLE-EXPERIENCE-VISUAL-AUTHENTICITY-CONTRACT.md`) applies for **Input-Coupled Motion**: when this Skill handles drag / swipe / scroll-linked / scrub / user-interruption of automatic movement, visual state must respond to current input / progress rather than stale queued destination animations. Motion Physics supplies retargetable, interruptible mechanics from the current visual state; the contract owns only the cross-cutting quality constraint, not interpolation / continuity / settling / easing / physical feel — those remain this Skill's ownership. Do NOT introduce fixed millisecond limits, spring values, velocity thresholds, or snap thresholds.

## Reference catalog (Tier 2)

- `references/spring-response.md` — spring response heuristics, amplitude ranges, perception.
- `references/retargeting.md` — retargeting from current state with code-shape examples.
- `references/input-sensitive-amplitude.md` — touch vs. mouse / trackpad vs. keyboard amplitude ranges.
- `references/reduced-motion.md` — reduced-motion adaptation rules.
- `references/cursor-rule.md` — pointer highlight motion must not affect the native cursor.

## Hard invariants (recap)

1. No fake Apple canonical numeric values (HARD).
2. Interruptibility (HARD).
3. Input-sensitive amplitude (HARD).
4. Reduced-motion preserves state, not "no motion" (HARD).
5. Framework-neutral (HARD).
6. Retargeting from current state, not "finish A then start B" (HARD).
7. The 7 stable v1.1 Skills are READ-ONLY.

## Companion files

- `references/spring-response.md`
- `references/retargeting.md`
- `references/input-sensitive-amplitude.md`
- `references/reduced-motion.md`
- `references/cursor-rule.md`
