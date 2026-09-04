---
name: apple-control-states
description: Use when designing or implementing the visual state model of an interactive custom control — what states a control has (idle / hover / pressed / focused / selected / disabled / loading / destructive), how the states transition, the cancellation requirement for press, the focus-vs-hover-vs-selection distinction, and the loading-state options. Required for custom control work on Apple-platform-inspired or Apple-platform-native UIs. Do NOT use for the motion physics of state transitions (use apple-motion-physics) or for the pointer-target relationship (use apple-pointer-interaction). Do NOT use for page-level composition.
version: "0.1.0-dev"
license: MIT
---

# apple-control-states

What states a control has. The state model is independent of how the state is reached (motion physics) and where the state came from (pointer / touch / keyboard).

## When to use

Use when designing or implementing a custom interactive control whose intent comes from Apple platform UX. Symptoms:

- "What states should this control have?"
- "The pressed state on hover looks like a hover state. They're not the same."
- "The disabled state still shows active hover. That's wrong."
- "After the user moves the pointer off the control while pressing, the control activates. That's wrong."
- "The loading state should differentiate from disabled."
- "Focus indicator looks like hover."

Do **not** use when:

- The task is motion physics (timing, spring, retargeting). Use `apple-motion-physics`.
- The task is the pointer-target relationship. Use `apple-pointer-interaction`.
- The task is page-level composition. Use the stable `apple-web-design`.
- The task is component grammar. Use the stable `apple-ui-components`.

## Hard invariants

1. **Interactive custom controls require perceptible press feedback.** A control with no visible press state is a defect. Press feedback may be implemented as scale, opacity, tint, highlight, material, shadow, icon state, or a combination. The exact value is a HEURISTIC, not an Apple canonical number.
2. **Press must include cancellation.** When the user moves the pointer or finger away from the control during a press, the control must return to its pre-press state without firing the activation. The lifecycle is `idle → pressed → (release inside) activate | (release outside) cancel → recovery → idle`.
3. **Disabled is a distinct state.** A disabled control must not show active hover, active press, animated selection, or clickable affordance. It must remain readable, accessible (`aria-disabled` / `disabled`), and visually distinct from enabled.
4. **Focus, hover, and selection are distinct states.** A Skill that uses `:hover` to indicate focus, or `selected` to indicate hover, is wrong. They are three separate states with separate visual treatments.
5. **Loading has variants.** Optimistic action, short asynchronous action (< 1 s), long-running action (with progress), and duplicate-submit prevention are different. A Skill that adds a spinner to every button on click is wrong.

## State model

A canonical control has the following state slots. Not every control uses every state. A control declares which states apply.

| State | Definition |
|---|---|
| `idle` | Default. No interaction. |
| `hover` | Pointer is over the control. Only applies when hover capability is available. |
| `pressed` | Pointer is down / finger is down on the control. Must include cancellation. |
| `focused` | Keyboard focus indicator is visible. Must be independent of hover. |
| `selected` | Persistent selected / active state. Survives pointer exit. Survives re-render. |
| `disabled` | Not interactive. No hover / press / selection treatment. |
| `loading` | An action is in progress. |
| `destructive` | The action is destructive (delete, remove, etc.). Often combined with confirmation. |

Not every control uses every state. A static label has no `hover` (it has no interactivity). A disabled submit button has no `pressed`. The state-availability matrix lives in `references/state-model.md`.

## State transitions

The canonical press lifecycle:

```text
idle
  → pointerdown / touchstart
  → pressed
  → release inside
  → activate
  → recovery
  → idle
```

The cancellation branch:

```text
pressed
  → pointerleave / pointercancel / touchcancel
  → cancel
  → recovery
  → idle
```

The selection lifecycle (for selectable controls):

```text
idle
  → click / Enter / Space
  → selected

selected
  → click / Enter / Space
  → idle (toggle off)
  → OR → selected (different option)
```

The loading lifecycle:

```text
idle
  → click
  → loading
  → (success)
  → idle (or selected if action persists)
  → (failure)
  → idle (with error indication)
```

## Focus / hover / selection distinct

Three states, three visual treatments. They MAY share a color family but they MUST have distinct visual cues.

- **focus** — accessibility-required, keyboard-driven, must be visible. Default browser focus ring is acceptable. If removed, an equivalent visible indicator is required.
- **hover** — pointer-driven, transient. No focus dependency.
- **selected** — persistent, the user's chosen state. Survives pointer exit.

A control that uses the same color / opacity / scale for any two of these states is wrong.

## Press feedback (HARD INVARIANT)

> A control with no perceptible press state is a defect.

The press feedback MAY be implemented as:

- scale (e.g. 0.97 — this is a heuristic, not Apple canonical)
- opacity (e.g. 0.85)
- tint / background-color shift
- highlight (a surface that appears on press)
- material change (e.g. a Liquid Glass surface intensifies on press)
- shadow / depth change
- icon state change (e.g. a chevron rotates)
- a combination of the above

The exact value is a HEURISTIC. Do NOT write "Apple uses scale 0.96" — that is unsupported.

The press feedback MUST include cancellation. A control that activates on every pointer-down regardless of where the pointer ends up is wrong.

## Disabled state (HARD INVARIANT)

A disabled control:

- MUST NOT show active hover treatment.
- MUST NOT show press treatment.
- MUST NOT animate selection.
- MUST NOT suggest clickability (cursor: not-allowed is appropriate).
- MUST remain readable (color contrast maintained).
- MUST remain accessible (`aria-disabled="true"` or `disabled` attribute).
- MUST be visually distinct from enabled (opacity, color, or other treatment).

## Loading state

A control can be in a loading state in one of:

- **Optimistic action** — UI updates immediately, server confirms later. Loading state is invisible to the user.
- **Short asynchronous action** (< 1 s) — show a subtle progress indicator or disable the control. Do NOT add a spinner for sub-second actions.
- **Long-running action** — show a progress indicator. Disable the control. Optionally show a cancel affordance.
- **Duplicate-submit prevention** — after the first activation, ignore further activations until the action completes.

A Skill that adds a spinner to every button on click is wrong. The button label change and reduced interactivity are usually sufficient for short actions.

## Reference catalog (Tier 2)

- `references/state-model.md` — full state-availability matrix by input modality.
- `references/press-lifecycle.md` — the press / cancel / activate lifecycle with code-shape examples.
- `references/focus-vs-hover-vs-selection.md` — three distinct states, visual treatment rules.
- `references/disabled-state.md` — disabled-state constraints.
- `references/loading-state.md` — loading-state variants and selection rules.
- `references/destructive-state.md` — destructive-action affordance and confirmation.

## Hard invariants (recap)

1. Perceptible press feedback (HARD).
2. Press cancellation (HARD).
3. Disabled does not show active interaction (HARD).
4. Focus ≠ hover ≠ selection (HARD).
5. Loading has variants (do not add spinner to every click).
6. Press feedback value is a HEURISTIC, not Apple canonical.
7. The 7 stable v1.1 Skills are READ-ONLY.

## Companion files

- `references/state-model.md`
- `references/press-lifecycle.md`
- `references/focus-vs-hover-vs-selection.md`
- `references/disabled-state.md`
- `references/loading-state.md`
- `references/destructive-state.md`
