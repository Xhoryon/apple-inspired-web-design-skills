# Hover / Focus / Selection Distinctness (Pointer-Interaction View)

The three states must have distinct visual treatments. This file is the Tier 2 reference for `apple-pointer-interaction`. The `apple-control-states` reference `focus-vs-hover-vs-selection.md` covers the same principle from the control-state angle; this file covers the pointer-target angle.

## Why they are distinct (pointer-target view)

| State | Driver | Pointer relevance | Required? |
|---|---|---|---|
| `focus` | Keyboard | Independent of pointer | YES (accessibility) |
| `hover` | Pointer with hover | The pointer IS the hover | NO (depends on input) |
| `selection` | User choice | Independent of pointer | NO (depends on control) |

A control that uses the same color, opacity, or scale for any two of these states is wrong. The user cannot distinguish them.

## Examples of conflation (wrong)

- Using `:hover` to indicate focus. A keyboard user navigating with Tab will not see the focus indicator.
- Using `selected` to indicate hover. When the pointer exits, the indicator disappears — but the user may have selected the control and expects the selected state to persist.
- Removing `:focus-visible` because the design has a hover state. The keyboard user is now blind to focus.

## Examples of distinct treatments (right)

- `focus` — a 2 px outline around the control, in the system accent color. The outline is visible without pointer state.
- `hover` — a 4% background-color shift, in a neutral color. The shift is transient; it disappears when the pointer exits.
- `selection` — a filled background, in the brand accent. The fill is persistent; it survives pointer exit.

A control that has all three may have three distinct visual treatments. They MAY share a color family (e.g. all in the same accent color) but they MUST have distinct visual cues (outline vs. shift vs. fill).

## Pointer highlight vs focus indicator

The Web pointer highlight (transient, follows the pointer) is distinct from the focus indicator (persistent, marks the focused element). They are NOT the same visual treatment.

A Skill that uses the pointer highlight as the focus indicator is wrong: a keyboard user navigating with Tab will not see the pointer highlight (no pointer is over the control).

A Skill that uses the focus indicator as the pointer highlight is wrong: the pointer highlight should retarget with the pointer; the focus indicator should not move when the pointer moves (it moves only when focus moves).

## Pointer highlight vs selected indicator

The Web pointer highlight (transient) is distinct from the Web selected indicator (persistent). They are NOT the same visual treatment.

A Skill that uses the pointer highlight as the selected indicator is wrong: when the pointer exits, the indicator disappears — but the user has selected the control and expects the selected state to persist.

A Skill that uses the selected indicator as the pointer highlight is wrong: the selected indicator is persistent; the pointer highlight is transient.

## Hover on touch

Hover does not apply to touch-only inputs. A Web Skill must gate the hover style by `(hover: hover)` media query. Touch-only devices do not receive the hover state. See `pointer-modality.md`.

## Selection on pointer exit

The selected state must persist after the pointer exits the control. The selected visual treatment must NOT be removed on `pointerleave` or `mouseleave`. The selected state is removed only when the user changes the selection (clicks again, or selects a different option in a group).

## Focus visibility on disabled controls

A disabled control may have its focus indicator visually distinct from an enabled control's focus indicator. The disabled control is still focusable (for accessibility) but its focus indicator may be dimmed or replaced with a "not interactive" treatment. The focus state itself still exists; the visual treatment may differ.

## Reduced motion vs focus visibility

A Skill that animates the focus indicator with a long motion is wrong. Focus must be visible. The focus indicator may have a brief transition (≤ 80 ms) but it must be visible immediately on focus, not fade in over 280 ms.

## Cross-Skill note

- The state catalog is `apple-control-states`.
- The pointer-target relationship is `apple-pointer-interaction` (this Skill).
- The control-state view of the same principle is `apple-control-states/references/focus-vs-hover-vs-selection.md`.
- The motion physics of the focus / hover / selection transitions is `apple-motion-physics`.
- The pointer modality gating is `pointer-modality.md`.
