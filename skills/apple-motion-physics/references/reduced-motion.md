# Reduced Motion Reference

`prefers-reduced-motion: reduce` is a user accessibility preference. The Skill reduces motion while preserving essential feedback. This file is the Tier 2 reference for `apple-motion-physics`.

## The rule

> `prefers-reduced-motion: reduce` must reduce or remove large motion while preserving essential feedback.

A Skill that removes all motion under reduced-motion preference is wrong. A Skill that ignores the preference is also wrong.

## What is "essential feedback"?

Essential feedback is the information the user needs to know that something happened. Examples:

- A control's color change on hover.
- A button's color change on activation.
- A focus outline appearing.
- A selection state being visible.
- A loading indicator (spinner or progress bar).

A Skill must keep essential feedback visible under reduced-motion preference.

## What is "non-essential motion"?

Non-essential motion is animation that adds visual flourish but does not communicate information. Examples:

- A large spring with overshoot on press feedback.
- A long translate or scale on hover.
- A long fade-in / fade-out on page transitions.
- A continuous subtle animation (e.g. a pulsing border).
- A bounce or wiggle effect.

A Skill must reduce or remove non-essential motion under reduced-motion preference.

## Reduction rules (heuristic)

These are HEURISTICS, not Apple canonical numeric values.

- **Duration**: from typical 240–280 ms to ≤ 80 ms.
- **Amplitude**: from typical 0.04 scale or 8 px translate to ≤ 0.01 scale or 2 px translate.
- **Translation**: from 8 px to 0 px (or ≤ 2 px).
- **Spring overshoot**: removed. Critical damping only.
- **Continuous animations**: removed (e.g. pulsing borders, fade-in / fade-out on long intervals).

The implementer tunes based on the project.

## What the user still sees under reduced-motion preference

- A button changes color on hover (or fades in over ≤ 80 ms).
- A focus outline appears.
- A loading spinner is still visible (the spinner is essential feedback that an action is in progress; the rotation is acceptable as it is not "large motion").
- A selection state is still visible.
- A press feedback is still present (color change or ≤ 0.01 scale change, not a 0.04 scale with overshoot).

## Implementation pattern (Web)

A common CSS pattern:

```css
.button {
  transition: transform 240ms var(--ease-spring), background-color 120ms ease;
  background-color: var(--bg-default);
  transform: scale(1);
}

.button:hover {
  background-color: var(--bg-hover);
}

.button:active {
  transform: scale(0.97);
}

@media (prefers-reduced-motion: reduce) {
  .button {
    transition: background-color 80ms ease;
  }
  .button:active {
    transform: scale(0.99);
  }
}
```

Under reduced-motion preference:
- Duration is reduced (240 ms → 80 ms).
- Amplitude is reduced (0.97 → 0.99).
- Color transitions remain.
- The press feedback is still visible (color + 0.01 scale change).

## A Skill that removes the press feedback entirely on reduced motion is wrong

A press feedback that is "the button changed color on press" is acceptable. A press feedback that is "the button shrank 4% with overshoot" is non-essential and should be reduced.

A Skill that says "on reduced motion, the button does not respond to press" is wrong. The state change is preserved; only the motion is reduced.

## Native Apple platforms

iOS / iPadOS / macOS respect the user's "Reduce Motion" accessibility setting. The system primitive respects the setting automatically. The Skill recommends the system primitive, not a custom re-implementation.

## Cross-Skill note

The reduced-motion rule applies to all motion in the new Skills (`apple-control-states`, `apple-pointer-interaction`, `apple-motion-physics`). This file is the canonical reference. The stable v1.1 `apple-motion-interaction/SKILL.md` and `apple-motion-interaction/references/reduced-motion.md` provide additional context for Web motion; the new Skills cite but do not duplicate them.
