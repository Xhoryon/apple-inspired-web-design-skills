# Focus and Dismissal Reference

Focus return on overlay dismissal, unsaved-change protection, focus entry, and screen-reader semantics. This file is the Tier 2 reference for `apple-modality-overlays`.

> **HARD RULE:** When an overlay closes, focus returns to the trigger element (or the next logical target). A modal that does not return focus is broken.

> **HARD RULE:** If the overlay contains unsaved destructive content, dismissal must be protected. The user must explicitly confirm the discard.

## Focus entry

When an overlay opens, focus moves to:

- The first focusable element in the overlay (the first form field, the first button, the title).
- A sensible default if no element is focusable (the overlay container itself, with `tabindex="-1"`).

The focus entry is:

- Predictable (the user expects the first field to be focused).
- Accessibility-friendly (the focused element is announced by screen readers).
- Visible (the focus indicator is clear).

## Focus containment (where required)

A modal overlay (sheet, alert, action sheet, popover) traps focus within the overlay. Tab / Shift+Tab cycles within the overlay; focus does NOT escape to the underlying hierarchy.

The focus trap is:

- Implemented by the Web framework (most dialog libraries provide it).
- Required for `role="dialog"` and `role="alertdialog"`.
- Bypassed only when the overlay is non-modal (e.g. a non-modal sheet on iOS 26+).

A modal that does not trap focus is broken. The user can Tab to elements outside the modal, which is confusing.

## Escape

On Web, Escape closes the overlay (where appropriate). The Skill records this as a hard invariant for Web modals.

- **Sheet (non-destructive):** Escape dismisses; focus returns to the trigger.
- **Sheet (destructive content):** Escape is intercepted; the unsaved-changes confirmation is shown.
- **Popover:** Escape dismisses; focus returns to the source.
- **Menu:** Escape dismisses; focus returns to the trigger.
- **Alert (alertdialog):** Escape may dismiss; some alerts require explicit button click. The implementation decides.
- **Action sheet (on touch Web):** Escape is not relevant (no keyboard). Tap outside dismisses.

## Explicit dismissal

An overlay has explicit dismissal affordances:

- **Done / Close / Cancel button.** The canonical dismissal.
- **Submit button** (e.g. "Save" on a sheet). The user commits; the sheet dismisses; the result is applied.
- **Discard button** (e.g. "Discard Changes" on a sheet with unsaved content). The user discards; the sheet dismisses; the result is discarded.

## Outside click / tap

On Web, a click on the backdrop dismisses the overlay (where appropriate). The Skill records this for non-destructive overlays.

- **Sheet (non-destructive):** backdrop click dismisses; focus returns to the trigger.
- **Sheet (destructive content):** backdrop click is intercepted; the unsaved-changes confirmation is shown.
- **Popover:** outside click dismisses; focus returns to the source.
- **Menu:** outside click dismisses; focus returns to the trigger.
- **Alert (alertdialog):** backdrop click is intercepted; the alert is not dismissed.
- **Action sheet (on touch Web):** backdrop tap dismisses; the choice is not made.

## Focus return

When an overlay closes, focus returns to:

- The element that triggered the overlay (the trigger element).
- If the trigger element no longer exists, focus moves to the next logical target (e.g. the next list item, the parent container).

The focus return is:

- Predictable (the user expects to return to the trigger).
- Accessibility-friendly (the screen reader announces the new focus).
- Visible (the focus indicator is clear on the returned-to element).

A modal that does not return focus is broken. The user is left at `<body>` with no idea where they came from.

## Cancellation

The user may cancel a task:

- **Cancel button.** The user explicitly cancels; the overlay dismisses; the result is discarded.
- **Escape key (Web).** Where appropriate, Escape cancels.
- **Backdrop tap (Web).** For non-destructive overlays, backdrop tap cancels.

The cancellation path is the same as the dismissal path. The result is discarded.

## Unsaved-change protection

If an overlay contains unsaved destructive content, dismissal must be protected. The user must explicitly confirm the discard.

The pattern:

1. The user attempts to dismiss (Done / Cancel / Escape / backdrop tap / swipe).
2. The system intercepts the dismissal and shows an unsaved-changes confirmation: "Discard changes?"
3. The user explicitly chooses:
   - **Discard** — the result is discarded; the overlay dismisses.
   - **Cancel** — the dismissal is cancelled; the user returns to the overlay.

The unsaved-change protection applies to:

- Sheets with form fields that have been modified.
- Sheets with destructive actions not yet committed.
- Alerts with destructive actions not yet confirmed (e.g. "Delete file? Yes / No").
- Any overlay with state that the user has invested effort in.

The unsaved-change protection does NOT apply to:

- Popovers that contain no state (e.g. a "Share" popover).
- Menus (commands, not state).
- Non-destructive overlays (e.g. a "View as" popover).

### Destructive state

A destructive state is one where the user's action has a permanent effect:

- Deleting a file.
- Sending a message.
- Submitting a form.
- Confirming a payment.

A destructive state must be protected by an explicit confirmation. The Skill rejects:

- A destructive action that can be triggered by a single accidental click without confirmation.
- A destructive action that is dismissed by Escape or backdrop tap without confirmation.

## What the Skill rejects

- A modal that does not return focus.
- A modal that does not trap focus.
- A modal that is dismissed by Escape without unsaved-change protection (when the modal has destructive state).
- A modal that destroys the user's work without confirmation.
- A modal that opens without focus entry (the user is left at the trigger, with no focus on the modal).

## Cross-Skill note

- The decision tree (when to use a modal) is `modality-decision-tree.md`.
- The sheet semantics, dismissal, focus return are `sheets.md`.
- The popover / menu distinction is `popovers-and-menus.md`.
- The alert / confirmation distinction is `alerts-and-confirmation.md`.
- The source continuity is `source-continuity.md`.
- The state semantics of focus are `apple-control-states`.
- The reduced-motion rule is `apple-motion-physics/references/reduced-motion.md`.
