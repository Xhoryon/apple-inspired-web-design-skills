# State Model and Availability Matrix

The state model and per-input-modality availability matrix for controls. This file is the Tier 2 reference for `apple-control-states` and `apple-experience-design`. It is the canonical matrix — both Skills cite this file.

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

A Skill that recommends a state must check this matrix. A Skill that uses a state on a non-matching input is wrong.

## Input → state rules

- **Mouse:** hover, pressed, focused (NOT by default; mouse click may focus on some platforms but the convention is "click focuses, hover does not"), selected, disabled, loading, destructive. Mouse does NOT focus on hover by default.
- **Trackpad:** same as mouse.
- **Touch:** pressed, focused (via accessibility tap or system), selected, disabled, loading, destructive. Touch does NOT have hover (no fine pointer; no hover state).
- **Keyboard:** focused, selected, disabled, loading, destructive. Keyboard does NOT have hover or pressed; Enter / Space activates a focused control.
- **Stylus / Pencil:** same as touch, with hover capability (when the stylus is near the screen). The hover state may apply.

## Native Apple behavior

- **iOS / iPadOS / macOS:** the system Button (UIButton, NSButton) provides idle, highlighted (hover / pressed merged), focused, selected, disabled. The system primitive handles the cancellation branch internally.
- **iPadOS 26+:** a separate hover effect may apply when a pointer is over a control (e.g. a trackpad / mouse on iPad). The system handles the visual effect.

## Web behavior

The Web behavior follows the matrix above with the additional gating:

```css
@media (hover: hover) and (pointer: fine) {
  /* Desktop Web, mouse / trackpad. Pointer effects apply. */
}
@media not (hover: hover) {
  /* Touch-only. Pointer effects do NOT apply. */
}
```

A Skill that recommends a pointer-specific behavior on Web MUST gate it by pointer modality. Touch-only devices must not receive hover effects.

## Disabled (recap)

A disabled control must NOT:

- show active hover treatment
- show press treatment
- animate selection
- suggest clickability

A disabled control MUST remain:

- readable
- accessible (`aria-disabled` / `disabled`)
- visually distinct from enabled

## Loading (recap)

A loading control may be in one of several states:

- Optimistic action (UI updates before server confirms).
- Short asynchronous action (< 1 s).
- Long-running action (with progress indicator).
- Duplicate-submit prevention (after first activation, ignore further activations).

Default: do NOT add a spinner to a button on click. The button label and reduced interactivity are usually sufficient for short actions.

## Cross-Skill note

- The press lifecycle (idle → pressed → activate → recovery → idle) is `press-lifecycle.md`.
- The focus / hover / selection distinctness is `focus-vs-hover-vs-selection.md`.
- The disabled-state constraints are `disabled-state.md`.
- The loading-state variants are `loading-state.md`.
- The destructive-state affordance is `destructive-state.md` (in `apple-control-states/references/`).
- The pointer-target relationship is `apple-pointer-interaction`.
