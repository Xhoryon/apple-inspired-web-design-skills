# Disabled State Reference

The disabled state is a distinct state with strict rules. This file is the Tier 2 reference for `apple-control-states`.

## Constraints (canonical)

A disabled control MUST NOT:

- Show active hover treatment.
- Show press treatment.
- Animate selection.
- Suggest clickability (`cursor: not-allowed` is appropriate).
- Animate on focus (focus is still required for accessibility, but the visual treatment may differ).

A disabled control MUST:

- Remain readable (color contrast maintained).
- Be accessible (`aria-disabled="true"` or `disabled` attribute).
- Be visually distinct from enabled (opacity, color, or other treatment).
- Still receive keyboard focus (for accessibility); the focus indicator may be dimmed or replaced with a "not interactive" treatment.

## Visual treatment (heuristic)

Common patterns:

- Reduced opacity (e.g. 0.4–0.5).
- Desaturated color.
- Lighter text color.
- No box-shadow or no highlight.

The exact value is a HEURISTIC. The Skill must NOT write "Apple disabled opacity 0.4" — that is unsupported.

## Press / hover cancellation

When a control becomes disabled mid-press:

- The press state is removed.
- The activation is NOT fired.
- The control transitions to disabled visual state.

This is a common bug: a user presses a control, the system disables the control (e.g. async validation says "you can't submit this"), and the control activates anyway. The correct behavior is: disable cancels any pending press / activation.

## Loading vs disabled

A loading control and a disabled control are distinct:

- **Loading** — an action is in progress. The user is waiting for the action to complete. The control may show a progress indicator.
- **Disabled** — the control is not interactive. The user cannot activate it.

A control that is loading is NOT disabled. The user may be able to cancel the action (depending on the control). A loading control retains its interactive affordance until the user explicitly cancels or the action completes.

A Skill that puts a loading control into the disabled state is wrong. The control should show a loading state and remain active (so the user can cancel).

## Selected vs disabled

A selected control and a disabled control are distinct:

- A **selected** control is active and persistent. The user has chosen it.
- A **disabled** control is not interactive.

A Skill that disables a selected control is wrong. Selection survives disabling the parent group, etc. Selection is per-option; disabling is per-option-or-per-group.

## Focus on disabled controls (accessibility)

A disabled control is still focusable (for accessibility) but the focus indicator may be visually distinct from an enabled control's focus indicator. The focus state itself still exists; the visual treatment may differ.

A Web implementation:

```css
button[disabled] {
  opacity: 0.5;
  cursor: not-allowed;
}
button[disabled]:focus-visible {
  outline: 2px solid var(--text-secondary);
}
```

The disabled control is focusable. The focus indicator is visible. The cursor indicates the control is not interactive. The opacity indicates the disabled state.

## Cross-Skill note

The disabled state is part of the state catalog (`apple-control-states`). The motion of the disabled transition is `apple-motion-physics`. The platform-applicability verdict for disabled on each platform is in `APPLE-EXPERIENCE-PLATFORM-MATRIX.md` (DIRECT on all current platforms).
