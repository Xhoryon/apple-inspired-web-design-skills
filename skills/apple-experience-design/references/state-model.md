# State Model and Availability Matrix

The state model is the abstract catalog of state slots a control can have. The state-availability matrix is the per-input-modality declaration of which slots apply.

## State slots

| State | Definition | Applies when |
|---|---|---|
| `idle` | Default. No interaction. | Always. |
| `hover` | Pointer is over the control. | Pointer device with hover capability. NOT touch-only. NOT keyboard. |
| `pressed` | Pointer-down / touch-down on the control. Must include cancellation. | Pointer / touch input. |
| `focused` | Keyboard focus indicator is visible. | Keyboard navigation. Always (accessibility). |
| `selected` | Persistent selected / active state. Survives pointer exit. | Selectable controls. |
| `disabled` | Not interactive. | When the control is disabled. |
| `loading` | An action is in progress. | When an action is in flight. |
| `destructive` | The action is destructive. | Destructive actions (delete, remove). |

Not every control uses every state. A static label has no `hover`. A disabled submit button has no `pressed`. A non-selectable display has no `selected`.

## State-availability matrix (canonical)

| State | Mouse | Trackpad | Touch | Keyboard | Native iOS / iPadOS / macOS |
|---|---|---|---|---|---|
| `idle` | YES | YES | YES | YES | YES |
| `hover` | YES | YES | NO | NO | depends on platform / input mode |
| `pressed` | YES (mousedown) | YES (trackpad press) | YES (touchstart) | NO (Enter / Space activate) | YES |
| `focused` | NO (mouse does not focus on hover) | NO (trackpad same) | NO (touch does not focus on tap unless interactive) | YES (Tab / Shift+Tab) | YES |
| `selected` | YES | YES | YES | YES | YES |
| `disabled` | YES | YES | YES | YES | YES |
| `loading` | YES | YES | YES | YES | YES |
| `destructive` | YES | YES | YES | YES | YES |

A Skill that recommends a state must populate the matrix above with the verdict for the target input modality. If a state is N/A for the target input, the Skill must say so and explain why.

## Press lifecycle (canonical)

```
idle
  → pointerdown / touchstart
  → pressed
  → release inside
  → activate
  → recovery
  → idle
```

Cancellation branch:

```
pressed
  → pointerleave / pointercancel / touchcancel
  → cancel
  → recovery
  → idle
```

This lifecycle is mandatory. A control that does not handle cancellation is not a real interactive control.

## Focus / hover / selection distinctness

These three states are distinct and must have distinct visual treatments. The state-availability matrix shows which inputs even produce each state (mouse produces hover but not focus; keyboard produces focus but not hover; touch produces neither).

A Skill that uses `:hover` to indicate focus is wrong. A Skill that uses `selected` to indicate hover is wrong. A Skill that uses the same visual treatment for any two of them is wrong.

## Disabled state (canonical rules)

- No active hover treatment.
- No press treatment.
- No selection animation.
- No clickable affordance.
- Maintain readability (color contrast).
- Maintain accessibility (`aria-disabled="true"` or `disabled`).
- Visually distinct from enabled (opacity, color, or other treatment).

## Loading state (canonical variants)

- **Optimistic action** — UI updates immediately. Loading state is invisible to the user.
- **Short asynchronous action** (< 1 s) — show a subtle progress indicator or disable the control. Do NOT add a spinner for sub-second actions.
- **Long-running action** — show a progress indicator. Disable the control. Optionally show a cancel affordance.
- **Duplicate-submit prevention** — after the first activation, ignore further activations until the action completes.

A Skill that adds a spinner to every button on click is wrong.

## Cross-Skill note

The state model is the semantic catalog. The motion physics of how states transition is `apple-motion-physics`. The pointer-target relationship for the `hover` state is `apple-pointer-interaction`. This Skill does not own motion or pointer; it owns the state catalog.
