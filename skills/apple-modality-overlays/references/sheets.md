# Sheets Reference

Sheet semantics, presentation, dismissal, focus return, unsaved-change protection. This file is the Tier 2 reference for `apple-modality-overlays`.

> **HARD RULE:** A sheet is for a focused, distinct task. Dismissing a sheet does not navigate back; the hierarchy behind the sheet is preserved.

> **VERSION-SENSITIVE:** iOS 26 sheet / dialog / presentation behaviors are tagged VERSION-SPECIFIC. The Skill does not generalize iOS 26 behaviors to all Apple platforms.

## Sheet definition

A sheet is a focused, distinct task that temporarily supersedes the current context. The sheet:

- Is presented over the current context.
- Has a presenting source (the element that triggered it).
- Has a presentation (the sheet itself, with content, title, controls).
- Has a detent / size (compact, medium, large; on iPadOS 26+).
- Has a dismissal (Done, Cancel, swipe-to-dismiss, tap outside, Escape on Web).
- Has a commit / cancel semantic.

The hierarchy behind the sheet is preserved. Dismissing the sheet does not navigate back; the user's previous context is intact.

## When a sheet is appropriate

- A focused, distinct task (edit a value, compose a message, review a list of items).
- A multi-step form.
- A presentation that requires the user's attention but is part of the workflow.
- A modal task that is bounded and discrete.

## When a sheet is NOT appropriate

- Success messages. (Use inline status / toast.)
- Generic confirmations. (Use alert or action sheet.)
- Supplementary information. (Use inline disclosure.)
- Long, complex tasks that should be navigation destinations. (Use a navigation destination with a URL.)
- Commands. (Use menu / context menu.)
- Unexpected information. (Use alert.)

## Size / detent (iOS 26+ — VERSION-SPECIFIC)

iPadOS 26+ supports sheet detents:

- **Compact** — small height; suitable for a quick choice.
- **Medium** — half-screen; suitable for a focused form.
- **Large** — full-screen; suitable for a multi-step task.

The detent choice is a design decision. The Skill does not publish canonical values; the implementer chooses based on the task. iOS 26+ supports a drag-to-resize gesture between detents.

iOS 26+ is VERSION-SPECIFIC. The Skill tags this behavior. On earlier iOS, the sheet is a fixed-size modal; the detent is not available.

## Presenting source

A sheet has a presenting source — the element that triggered it. The source is:

- The focus-return target on dismissal.
- The visual anchor for source-continuity (geometry, identity).
- The owner of the sheet's commit / cancel semantic.

A sheet without a presenting source is a defect. The user does not know what triggered it or where the result will be applied.

## Interactive content

A sheet may contain interactive content (form fields, controls, lists). The sheet:

- Scrolls independently of the underlying hierarchy.
- Has its own focus order (Tab cycles within the sheet).
- Has its own keyboard handling (Enter submits; Esc dismisses).
- Does NOT allow the user to interact with the underlying hierarchy while the sheet is open (modal).

A sheet that allows the user to interact with the underlying hierarchy is NOT a sheet; it is a non-modal presentation (which is a separate pattern; the Skill records this distinction).

## Dismissal

A sheet may be dismissed by:

- **Explicit Done / Close button.** The canonical dismissal for a completed task. The button label is "Done" or "Close" depending on the context.
- **Explicit Cancel button.** The user explicitly cancels the task. Unsaved changes are protected (see below).
- **Swipe-to-dismiss gesture (iOS 26+ on some sheet types).** VERSION-SPECIFIC. The user swipes the sheet down; the sheet animates out. Web may ADAPT (custom drag-to-dismiss).
- **Tap on the backdrop (Web and some iOS sheet types).** On Web, a click on the backdrop dismisses the sheet. On iOS, the behavior depends on the sheet type (compact sheets may dismiss on backdrop tap; full-screen sheets may not).
- **System Back gesture (iOS — but only if the sheet is in the navigation stack).** In iOS 26, a non-modal sheet may not respond to Back. The Skill tags this.
- **Escape on Web (keyboard).** The Web standard. The sheet is dismissed; focus returns to the trigger.

### Dismissal and unsaved changes

A sheet that contains unsaved destructive content must require explicit dismissal. The user must explicitly click "Done" or "Cancel"; the sheet may NOT be dismissed by swipe, backdrop tap, or Escape without confirmation.

The pattern:

1. The user attempts to dismiss (swipe, backdrop, Escape).
2. The system intercepts the dismissal and shows an unsaved-changes confirmation: "Discard changes?"
3. The user explicitly discards or cancels the dismissal.

The system is `apple-modality-overlays`; the unsaved-change protocol is `focus-and-dismissal.md`.

## Done / Cancel

A sheet has Done and Cancel semantics:

- **Done** — the user commits the task. The sheet dismisses; the result is applied to the source.
- **Cancel** — the user cancels the task. The sheet dismisses; the result is discarded.

The button labels are "Done" and "Cancel" (or context-specific equivalents: "Save", "Close", "Send"). The Skill does not publish canonical labels; the implementer chooses based on the task.

## Web approximation boundary

On Web, a sheet is a dialog with focus trap, backdrop, and Escape handling. The Web sheet:

- Uses `role="dialog"` (or `role="alertdialog"` for critical sheets).
- Traps focus within the sheet (Tab / Shift+Tab cycles within).
- Returns focus to the trigger on dismiss.
- Closes on Escape.
- Has a backdrop that closes the sheet on click (for non-destructive sheets).
- Provides screen-reader semantics (`aria-modal`, `aria-labelledby`, `aria-describedby`).

The Web sheet is an APPROXIMATION of the iOS sheet. The Skill tags this. The Web sheet does not have the same animation, the same detents, or the same system Back gesture; it is a dialog.

The Web sheet is acceptable for Web tasks. The Skill does not recommend implementing iOS-specific detents on Web; the Web sheet is one of several presentation choices (modal, side panel, etc.).

## iOS 26+ source-presentation continuity (VERSION-SPECIFIC)

iOS 26 introduces stronger source-presentation continuity for sheets:

- The sheet emerges from the trigger element's geometry.
- The sheet's content identity matches the trigger (label, icon).
- The dismissal returns to the trigger with the same geometry.

This is VERSION-SPECIFIC. The Skill does not generalize it to all Apple platforms. Web approximation is HEURISTIC.

## Native Apple primitives

- **iOS / iPadOS:** `UISheetPresentationController` (UIKit) or `.sheet` (SwiftUI).
- **macOS:** `NSWindow` as a sheet, or `.sheet` (SwiftUI).

The Skill recommends the system primitive. Custom modal is rarely justified on Apple platforms.

## Cross-Skill note

- The decision tree (when to use a sheet) is `modality-decision-tree.md`.
- The popover / menu distinction is `popovers-and-menus.md`.
- The alert / confirmation distinction is `alerts-and-confirmation.md`.
- The focus return and unsaved-change protection are `focus-and-dismissal.md`.
- The source continuity and anchoring are `source-continuity.md`.
- The motion physics of sheet entry / dismiss is `apple-motion-physics`.
- The drag-to-dismiss gesture is `apple-direct-manipulation` (cited).
