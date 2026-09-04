# Modality Decision Tree Reference

The canonical modality decision model. This file is the Tier 2 reference for `apple-modality-overlays`.

> **HARD RULE:** Does this content actually need modality? If not, do not use a modal surface. Modal is for tasks that need focused attention.

## The first question

Before choosing a modal type, ask:

> Does this content actually need modality?

If the content is supplementary (an explanation, a hint, a tooltip, a non-blocking status), an inline disclosure is better. If the content is a confirmation of a small, well-understood action, an inline confirmation may be better. Modality is justified only when the content requires the user's focused attention AND the interruption is acceptable.

## The decision tree

The decision tree is a series of questions. The first YES narrows the choice.

| Question | YES → | NO → |
|---|---|---|
| Is the content supplementary or required? | If supplementary, use inline disclosure. | Continue. |
| Is this a focused, distinct task? | Sheet | Continue. |
| Is this a contextual action on a specific element? | Popover (Desktop Web), action sheet / context menu (touch) | Continue. |
| Is this a destructive action that the user might not intend? | Confirmation (alertdialog for critical / irreversible; action sheet for non-critical) | Continue. |
| Is this a command menu? | Menu / context menu | Continue. |
| Is this unexpected information the user must see? | Alert | Continue. |
| (No answer fits.) | Reconsider: do you need modality at all? | |

The tree favors the least-modal option. Modality is a cost; the user must dismiss the surface to return to their task.

## Decision tree in narrative form

### 1. Is the content supplementary or required?

- **Supplementary** (a hint, an explanation, a status update) → **inline disclosure** (popover, tooltip, expandable section, inline text). NOT a modal.
- **Required** (the user must see this to continue) → continue.

### 2. Is this a focused, distinct task?

- **YES** → **sheet**. A sheet is appropriate for editing a value, composing a message, reviewing a multi-step form. The sheet is a focused task; the hierarchy behind it is preserved; dismissing the sheet returns to the source.
- **NO** → continue.

### 3. Is this a contextual action on a specific element?

- **YES** → **popover** (Desktop Web) or **action sheet / context menu** (touch). The popover is anchored to the source; the action sheet is a list of actions tied to the user's recent action. The popover / action sheet is dismissable; it does not navigate.
- **NO** → continue.

### 4. Is this a destructive action that the user might not intend?

- **YES** → **confirmation**. The confirmation is:
  - **Alert** (alertdialog) for critical / irreversible actions (delete account, delete data without recovery, send a payment).
  - **Action sheet** for non-critical / reversible actions (archive, mark as read, move to a folder).
  - **Inline confirmation** for very small actions (e.g. toggle a setting).
- **NO** → continue.

### 5. Is this a command menu?

- **YES** → **menu / context menu**. The menu is a list of commands. It is not a form. It is dismissable.
- **NO** → continue.

### 6. Is this unexpected information the user must see?

- **YES** → **alert**. The alert is for important or unexpected information. It is not for success messages.
- **NO** → continue.

### 7. (No answer fits.)

The content does not need modality. Reconsider: use an inline disclosure, a navigation destination, or a non-modal surface.

## Reject Modal Matryoshka

Modal Matryoshka is the anti-pattern of stacking modal surfaces:

- Sheet inside a sheet.
- Popover inside a sheet.
- Alert inside a popover.
- Sheet → popover → alert.

Each layer reduces clarity. The user cannot tell which surface is the primary task. The dismissal path becomes ambiguous.

The Skill rejects Modal Matryoshka. If the content requires a stacked modal, the design is wrong; reconsider the structure.

## When to use each modality type — examples

| Content | Verdict | Type |
|---|---|---|
| "Tap to learn more about this feature." | Supplementary | Inline disclosure / tooltip |
| "Edit this field." | Focused task | Sheet |
| "What would you like to do with this file?" | Contextual action | Popover / action sheet |
| "Delete this file?" (reversible) | Non-critical confirmation | Action sheet / inline confirm |
| "Delete your account? This cannot be undone." | Critical confirmation | Alert (alertdialog) |
| "The network is unavailable." | Unexpected information | Alert |
| "Format > Bold" | Command | Menu / context menu |
| "The file was saved." | Non-blocking status | Toast / inline status, NOT an alert |
| "Open this deep link." | Major destination | Navigation (not a modal) |

## When to use navigation instead of modality

Some content that looks like a modal is actually an important independent destination:

- A page with a deep link.
- A page in browser history.
- A long, complex task.
- A primary destination for a user flow.

In these cases, the content should be a navigation destination (URL + back button), NOT a modal. A modal that is hard to link, hard to bookmark, or contains a long task is a misuse.

The URL / modal boundary:

- **Modal:** ephemeral, no URL, no history entry, dismissed on close.
- **Navigation destination:** persistent, has a URL, has a back affordance, preserved on close.

A "modal" that should be a navigation destination is a design defect. The Skill rejects this misuse.

## Source continuity and source-trigger relationship

A modal has a source — the element that initiated it. The source is:

- The button that opened the sheet.
- The element that the popover is anchored to.
- The action that triggered the alert.

The source is the focus-return target on dismissal. The source is also the visual anchor for source-continuity (geometry, identity). See `source-continuity.md`.

## Cross-Skill note

- The sheet semantics, dismissal, focus return are `sheets.md`.
- The popover / menu semantics are `popovers-and-menus.md`.
- The alert / confirmation distinction is `alerts-and-confirmation.md`.
- The focus return and unsaved-change protection are `focus-and-dismissal.md`.
- The source continuity and anchoring are `source-continuity.md`.
- The decision tree is the canonical model for this Skill; the implementation uses the platform primitive.
