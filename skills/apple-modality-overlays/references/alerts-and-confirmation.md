# Alerts and Confirmation Reference

The distinction between alert, confirmation, action sheet, and non-blocking status feedback. This file is the Tier 2 reference for `apple-modality-overlays`.

> **HARD RULE:** Alerts are for important or unexpected information. They are NOT for success messages.

> **HARD RULE:** Action sheets are for user-initiated choices. They are NOT a generic confirmation.

## The four surface types

| Surface | Trigger | Purpose | Severity |
|---|---|---|---|
| **Alert** | Unexpected information / critical decision | Stop the user; require acknowledgment | Important / unexpected / critical |
| **Action sheet** | User-initiated action | Present choices that follow from the user's action | Normal / reversible |
| **Confirmation** | Destructive or critical action | Confirm before committing | Varies |
| **Non-blocking status** | Background event | Inform the user without interrupting | Low |

These are NOT interchangeable.

## Alert

An alert is for:

- **Important or unexpected information.** "The network is unavailable. Your changes have not been saved."
- **A critical decision that the user must make.** "Delete your account? This cannot be undone."

An alert is NOT for:

- **Success messages.** "File saved." → use inline status / toast.
- **Generic confirmations.** "Are you sure?" → use a confirmation or an action sheet.
- **Multi-step flows.** "Step 1 of 3..." → use a sheet or a navigation destination.
- **Non-critical information.** "New version available." → use a non-modal banner.

### Alert on Native iOS / iPadOS / macOS

The system primitive is `UIAlertController` (UIKit) or `.alert` (SwiftUI). The alert:

- Has a title.
- Has a message.
- Has one or two action buttons (e.g. "Cancel" and "Delete").
- Traps focus.
- Returns focus to the trigger on dismiss (or to the system's focus order).

### Alert on Web

The Web alert uses `role="alertdialog"` and a focus trap. The alert:

- Has a title (`aria-labelledby`).
- Has a message (`aria-describedby`).
- Has one or two action buttons.
- Traps focus within the dialog.
- Returns focus to the trigger on dismiss.
- Closes on Escape.
- Provides a backdrop.

The Web alert is a dialog with `role="alertdialog"`. The Skill does not use the browser's native `window.alert()`; that is not styleable, not localizable, and not accessible.

## Confirmation

A confirmation is a small set of choices (often binary: confirm / cancel) that the user must commit to. A destructive action is a common trigger.

A confirmation may be:

- **An alert (alertdialog)** for critical / irreversible actions. The alert has "Cancel" and "Delete" (or similar). The user must explicitly choose.
- **An action sheet (or popover)** for non-critical / reversible actions. The action sheet has the choices; the user picks one.

The choice depends on the severity:

| Severity | Surface |
|---|---|
| Irreversible, destructive, expensive | Alert |
| Reversible, non-destructive, low cost | Action sheet / inline confirm |
| Trivial, low cost | Inline confirm (e.g. "Are you sure?" in text) |

The Skill records this. A confirmation that uses an alert for a trivial action is over-moded; a confirmation that uses an inline confirm for a destructive action is under-moded.

## Action sheet

An action sheet is for **choices tied to a user-initiated action**. The user has done something (e.g. tapped "Share"), and the action sheet presents the choices that follow (e.g. "Share via Mail", "Share via Messages", "Copy Link").

An action sheet is distinct from an alert:

| Property | Alert | Action sheet |
|---|---|---|
| Trigger | Unexpected / critical | User-initiated action |
| Tone | "Stop. You must decide." | "You did X. What next?" |
| Cancel | Implicit (close without choosing) | Explicit ("Cancel" button) |
| Severity | High (critical) | Low (reversible) |

The Skill records this distinction. Apple HIG explicitly advises treating these as separate.

### Action sheet on iOS / iPadOS

The system primitive is `UIAlertController` with `.actionSheet` style (UIKit) or `.confirmationDialog` (SwiftUI).

### Action sheet on macOS / Desktop Web

The system primitive is a popover or a menu. The user picks an item; the action sheet dismisses.

### Action sheet on Touch Web

A bottom sheet is the touch-appropriate presentation. The user picks an item; the sheet dismisses.

## Reject common misuses

### Blocking alert for ordinary success messages

"File saved." is a success message. An alert that blocks the user until they click "OK" is wrong. Use a non-blocking status (toast, inline status, transient banner).

### Complex form in an alert

An alert that contains a form (multiple fields, a long list) is wrong. Use a sheet.

### Unnecessary confirmation everywhere

Confirming every action ("Are you sure you want to click this button?") is annoying. The user expects the action to happen; the confirmation is unnecessary friction.

The Skill recommends:

- **Confirm destructive actions.** "Delete this file?" is acceptable for a destructive action.
- **Don't confirm trivial actions.** "Click the button to view the page" — no confirmation.
- **Don't confirm reversible actions.** "Move this file to the trash?" — the user can undo (move from trash to file). No confirmation.

## Non-blocking status feedback

For non-critical information that the user should be aware of but does not need to act on:

- **Toast** — a transient message that appears and disappears.
- **Inline status** — a status line in the UI.
- **Banner** — a non-modal banner at the top of the page.

These are NOT alerts. They do not require acknowledgment; they do not block the user; they do not interrupt the task.

Examples:

- "File saved." → toast.
- "Syncing..." → inline status.
- "New version available." → banner with "Reload" action.

## What the Skill rejects

- An alert for a success message.
- An alert for a non-critical information.
- A confirmation for a trivial action.
- A confirmation for a reversible action.
- A complex form in an alert.
- A stacked modal (alert inside a popover, etc.).
- A blocking alert that the user cannot dismiss without a specific action (e.g. a forced sign-in alert that has no "Cancel").

## Cross-Skill note

- The decision tree (when to use alert vs action sheet) is `modality-decision-tree.md`.
- The sheet semantics are `sheets.md`.
- The popover / menu distinction is `popovers-and-menus.md`.
- The focus return is `focus-and-dismissal.md`.
- The source continuity is `source-continuity.md`.
- The unsaved-change protection is `focus-and-dismissal.md`.
