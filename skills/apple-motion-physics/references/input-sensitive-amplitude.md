# Input-Sensitive Amplitude Reference

The amplitude of interaction motion should be tuned to the input modality. This file is the Tier 2 reference for `apple-motion-physics`.

> **EVIDENCE-CLASS STATEMENT:** Apple HIG does not publish canonical numeric amplitude values. The values in this file are HEURISTICS.

## Why amplitude is input-sensitive

Different input modalities have different perceptual characteristics:

- **Touch** — coarse pointer; the entire surface is the touch area; the user expects immediate, visceral feedback.
- **Mouse / trackpad** — fine pointer; the user expects precise, controlled feedback.
- **Keyboard** — no pointer; the user expects immediate, clear state change.
- **Stylus / Pencil** — fine pointer with pressure; the user expects nuanced, pressure-sensitive feedback.

The amplitude of the press feedback, the hover effect, and the transition motion should be tuned to the modality. A press feedback that is 0.97 scale on touch and 0.99 scale on mouse is a reasonable starting point — but the implementer tunes based on the design.

## Touch

Touch surfaces are large. The user can see a large press feedback (e.g. scale 0.94–0.98) and still maintain visual contact with the rest of the UI.

- Press feedback amplitude: scale 0.94–0.98, opacity 0.7–0.9 (HEURISTIC).
- Press feedback duration: 60–100 ms (HEURISTIC).
- Release / recovery: 200–300 ms (HEURISTIC).
- Hover effect: not applicable (touch has no hover).

## Mouse / trackpad

Mouse and trackpad are fine pointers. The user expects precise, controlled feedback. A large press feedback is disorienting.

- Press feedback amplitude: scale 0.97–0.99, opacity 0.85–0.95 (HEURISTIC).
- Press feedback duration: 80–120 ms (HEURISTIC).
- Release / recovery: 200–300 ms (HEURISTIC).
- Hover effect: subtle background shift; 120–180 ms (HEURISTIC).

## Keyboard

Keyboard has no pointer. The user expects immediate state change. The "press" is the Enter / Space activation, not a continuous state.

- Press feedback: not a continuous state; activation is a discrete event.
- State change duration: ≤ 80 ms (HEURISTIC).
- Focus indicator: 2 px outline; visible immediately.

## Stylus / Pencil

Stylus is a fine pointer with pressure. The user expects nuanced, pressure-sensitive feedback.

- Press feedback amplitude: similar to mouse; may be modulated by pressure.
- Pressure-sensitive features: drawing, scrubbing, sliding scale.
- Hover: yes (the stylus is near the screen); subtle highlight.

## Apple Pencil on iPadOS

The Apple Pencil on iPadOS is the canonical stylus case. The Pencil has:

- Pressure (light, medium, hard).
- Tilt (angle of the Pencil).
- Hover (when the Pencil is near the screen, on supported hardware).

A Skill that recommends Pencil-specific behavior must tag the version (Pencil hover is iPadOS 16+ for second-generation Pencil; iPadOS 17+ for hover). The version sensitivity is HARD.

## Heuristic amplitude ranges (NOT Apple canonical)

These are starting points for tuning. They are not Apple canonical.

| Modality | Press duration | Press amplitude | Release duration | Hover duration |
|---|---|---|---|---|
| Touch | 60–100 ms | scale 0.94–0.98, opacity 0.7–0.9 | 200–300 ms | n/a |
| Mouse / trackpad | 80–120 ms | scale 0.97–0.99, opacity 0.85–0.95 | 200–300 ms | 120–180 ms |
| Keyboard | ≤ 80 ms | state change (no continuous) | n/a | n/a |
| Stylus | similar to mouse | similar to mouse | similar to mouse | subtle highlight |

The implementer tunes these based on:

- The device's performance envelope.
- The desired feel (snappy vs. soft).
- The user population (low-power devices may need longer durations).
- Reduced-motion preference.

## Reduced motion

`prefers-reduced-motion: reduce` reduces duration and amplitude. The state transition still happens; the motion is shorter and faster. The amplitude may be reduced to a minimum (e.g. opacity 0.95 instead of 0.85 for press feedback). The state change is preserved; the motion is reduced.

## Cross-Skill note

- The state semantics are in `apple-control-states`.
- The pointer-target relationship is in `apple-pointer-interaction`.
- The retargeting rule is `retargeting.md`.
- The reduced-motion rule is `reduced-motion.md`.
- The pointer highlight motion must not affect the native cursor is `cursor-rule.md`.
