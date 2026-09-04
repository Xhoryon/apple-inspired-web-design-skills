# Spring Response Reference

Spring response is the primary motion model for Apple-platform interaction transitions. This file is the Tier 2 reference for `apple-motion-physics`.

## The Skill does not publish canonical numeric values

The Apple HIG does not publish canonical spring constants for third-party implementations. A Skill that writes "Apple uses spring stiffness 320, damping 30" is unsupported and misleading.

The values in this file are HEURISTICS. The implementer tunes them based on the platform, the device, and the desired feel.

## Spring concept

A spring is a damped harmonic oscillator. The canonical spring equation is:

```
F = -k * x - c * v
```

where:
- `F` is the force (acceleration direction).
- `k` is the spring stiffness (how quickly the spring pulls toward the target).
- `x` is the displacement from the target.
- `c` is the damping coefficient.
- `v` is the velocity.

For UI motion, the relevant parameters are:

- **Stiffness** (k) — how quickly the motion approaches the target.
- **Damping** (c) — how quickly the motion settles.
- **Initial velocity** (v₀) — whether the motion is launched from rest or from a previous state.
- **Mass** (m) — usually 1 in UI motion.

## Damping regimes

- **Critical damping** — the motion reaches the target without overshoot. `c = 2 * sqrt(k * m)`.
- **Under-damping** — the motion overshoots the target, oscillates, and settles. `c < 2 * sqrt(k * m)`.
- **Over-damping** — the motion approaches the target slowly without overshoot. `c > 2 * sqrt(k * m)`.

For UI motion:

- **Press feedback** — usually slightly under-damped (small overshoot feels "alive").
- **Release / recovery** — usually critical or slightly under-damped.
- **Highlight retargeting (iPadOS 26-style)** — usually critical or near-critical damping (the highlight "settles" on the new target).
- **Page transitions** — not in scope of this Skill.

These are HEURISTICS. Tune per project.

## Heuristic amplitude ranges (NOT Apple canonical)

These are starting points for tuning. They are not Apple canonical. They are based on perceptual feedback and platform conventions.

| Transition | Recommended duration | Recommended amplitude |
|---|---|---|
| Press feedback (mouse / trackpad) | 80–120 ms | scale 0.97–0.99, opacity 0.85–0.95 |
| Press feedback (touch) | 60–100 ms | scale 0.94–0.98, opacity 0.7–0.9 |
| Release / recovery | 200–300 ms | back to 1.0 / 1.0 (rest) |
| Hover (mouse / trackpad) | 120–180 ms | background shift, scale 1.0 (no scale) |
| Shared highlight retarget | 200–280 ms | transform from current state to new state |
| Focus indicator | ≤ 80 ms | outline 2 px |
| Loading spinner | 800–1200 ms (rotation) | n/a |
| Page transition | not in scope | n/a |

The implementer tunes these based on:
- The device's performance envelope.
- The desired feel (snappy vs. soft).
- The user population (low-power devices may need longer durations).
- Reduced-motion preference (see `reduced-motion.md`).

## Velocity and continuity

A motion with non-zero initial velocity continues smoothly. A motion that resets to zero velocity at the target loses visual continuity.

For example, a press feedback that uses `transition: transform 100ms` (velocity goes to 0 at the end of the transition) is acceptable for a discrete press. A press feedback that uses a spring (velocity continues into the release) feels more alive.

The choice between discrete transition and spring is a design decision. Both are valid. The Skill recommends using a spring for the press / release / highlight-retarget sequence, and a discrete transition for state changes (focus, loading, success / error indication).

## Interruption

When input changes the target mid-motion, the motion should follow the new target from the current state (see `retargeting.md`). The motion does NOT finish the current animation and start a new one.

## Reduced motion

`prefers-reduced-motion: reduce` reduces duration and amplitude (see `reduced-motion.md`). The state transition still happens; the motion is shorter and faster.

## Cross-Skill note

The spring response is a physics concept. The state semantics are in `apple-control-states`. The pointer-target relationship is in `apple-pointer-interaction`. This Skill owns the physics, not the state catalog.
