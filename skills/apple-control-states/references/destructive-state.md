# Destructive State Reference

The destructive state is a state slot on a control that performs a destructive action. This file is the Tier 2 reference for `apple-control-states`.

## Destructive definition

A destructive action is one whose effect cannot be easily reversed:

- Delete a file / record / account.
- Remove an item from a list.
- Send a message (irreversible after send).
- Submit a payment (irreversible after submit).
- Discard unsaved work.

A destructive action must:

- Be visually distinct from a non-destructive action (a "Delete" button is colored differently from a "Save" button).
- Be confirmed (or be reversible).
- Have an alternative input (visible button, not just a gesture).

## Visual treatment

The destructive state visual treatment is a convention, not a strict rule:

- **Color:** red, or the brand's "destructive" color. The exact color is a brand decision.
- **Weight:** bold text, filled background, or a stronger border.
- **Icon:** a trash icon, an "X" icon, or a warning icon.
- **Position:** the destructive action is not the primary action; it is secondary (a button to the left of the primary action, or below the primary action).

A destructive action that looks identical to a non-destructive action is wrong. The user cannot tell them apart.

## Confirmation

A destructive action that is irreversible must require explicit confirmation:

- **Alert (alertdialog)** for critical / irreversible actions. The alert has "Cancel" and "Delete" (or similar). The user must explicitly choose.
- **Action sheet** for non-critical / reversible destructive actions. The action sheet has the choices; the user picks one.
- **Inline confirm** for trivial destructive actions (e.g. "Delete this draft?" inline in the form).

The confirmation level depends on the severity. A confirmation that uses an alert for a trivial destructive action is over-moded; a confirmation that uses an inline confirm for a critical destructive action is under-moded.

## Undo affordance

Where possible, provide an undo affordance:

- A "Trash" folder that the user can recover from.
- An "Undo" button after the action.
- An autosave that preserves the previous state.

The undo affordance is the canonical Apple HIG pattern. The user can recover from the destructive action without an explicit confirmation. The Skill recommends the undo affordance over the explicit confirmation where the undo is feasible.

## Disabled vs destructive

A disabled control is not interactive. A destructive control is interactive but the action is destructive. The two states are distinct:

- A disabled "Delete" button: not interactive. The user cannot click it.
- A destructive "Delete" button: interactive. The user can click it. The action is destructive; the visual treatment is distinct.

A control that is both disabled and destructive is a contradiction. The control is disabled, so the destructive state is not user-reachable.

## Cross-Skill note

- The state-availability matrix is `state-model.md`.
- The disabled-state constraints are `disabled-state.md`.
- The loading-state variants are `loading-state.md`.
- The unsaved-change protection (for destructive state in modals) is `apple-modality-overlays/references/focus-and-dismissal.md`.
- The alert / confirmation distinction is `apple-modality-overlays/references/alerts-and-confirmation.md`.
