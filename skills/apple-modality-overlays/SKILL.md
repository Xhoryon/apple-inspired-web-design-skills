---
name: apple-modality-overlays
description: Use when designing or implementing temporary surfaces and context interruption — sheet, popover, menu, context menu, alert, action sheet, confirmation, and the modality decision tree. Applies to Native iOS / iPadOS / macOS (system primitives preferred) and Desktop Web / Touch Web (focus trap, ARIA, dialog). Required for "should this be modal?", sheet / popover / menu selection, dismissal semantics, focus return, and unsaved-change protection. Do NOT use for control state semantics (use apple-control-states), pointer-target relationship (use apple-pointer-interaction), motion physics (use apple-motion-physics), gesture recognition (use apple-direct-manipulation), or navigation hierarchy (use apple-navigation-spatial). Do NOT stack unnecessary modal surfaces. Do NOT use alerts for success messages.
version: "0.1.0-dev"
license: MIT
---

# apple-modality-overlays

When the user needs a temporary surface and whether the content should interrupt the current context. The Skill owns modality decision, presentation taxonomy, dismissal semantics, focus return, and unsaved-change protection.

## When to use

Use when the task involves a temporary surface, a contextual action surface, or a context interruption. Symptoms:

- "When the user clicks the button, a sheet should open."
- "Right-click should show a context menu."
- "This action is destructive — should it be confirmed?"
- "When the user dismisses the sheet, focus should return to the button."
- "If the user has unsaved changes, dismissal should be protected."
- "The popover should anchor to the source and become a sheet on narrow viewports."
- "Don't use an alert for success messages."

Do **not** use when:

- The task is control state semantics (idle / hover / pressed / focused / selected / disabled). Use `apple-control-states`.
- The task is pointer-target relationship (hover highlight, shared highlight). Use `apple-pointer-interaction`.
- The task is motion physics (sheet entry / dismiss animation). Use `apple-motion-physics`.
- The task is gesture recognition (drag-to-dismiss). Use `apple-direct-manipulation`.
- The task is navigation hierarchy (push, peer, back). Use `apple-navigation-spatial`.

## Hard invariants

1. **First question: does this need modality at all?** Modal is for tasks that need focused attention. If the content is supplementary, an inline disclosure may be better.
2. **System primitive first.** On Native iOS / iPadOS / macOS, use UISheetPresentationController / SwiftUI .sheet, UIPopoverPresentationController, UIContextMenuInteraction, UIAlertController. Custom modal is rarely justified.
3. **No alert for success messages.** Alerts are for important or unexpected information. Success messages are not.
4. **No modal matryoshka.** A sheet → another sheet → popover → alert stack is bad. Each layer reduces clarity.
5. **Focus return on dismiss.** When an overlay closes, focus returns to the trigger element (or the next logical target).
6. **Unsaved-change protection.** A modal that destroys unsaved destructive content must require explicit confirmation, autosave, or a prevent-dismiss mechanism.
7. **iOS 26+ is VERSION-SPECIFIC.** Some iOS 26 sheet / dialog / presentation behaviors are new. The Skill tags these explicitly.
8. **Source continuity.** A popover or contextual surface is anchored to its source. The relationship is visible (geometry, anchor, visual continuity).

## Modality decision tree

Before choosing a modal type, answer:

1. **Is this content supplementary or required?** Supplementary → inline disclosure. Required → next question.
2. **Is this a focused distinct task?** Yes → sheet. No → next question.
3. **Is this a contextual action on a specific element?** Yes → popover (Desktop) or action sheet / context menu (touch). No → next question.
4. **Is this a destructive action that the user might not intend?** Yes → confirmation / alert (depending on severity). No → next question.
5. **Is this a command menu?** Yes → menu / context menu. No → next question.
6. **Is this unexpected information the user must see?** Yes → alert. No → reconsider whether modal is needed.

The answer at each step narrows the choice. The Skill recommends the least-modal option that satisfies the requirement.

## Presentation taxonomy

| Type | When | System primitive (Native Apple) | Web pattern |
|---|---|---|---|
| **Menu / context menu** | Commands / actions on the current selection or surface | UIContextMenuInteraction, NSMenu | Right-click (mouse), long-press (touch), keyboard shortcut |
| **Popover** | Contextual supplementary UI anchored to a source | UIPopoverPresentationController | Anchored element + ARIA; adapt to sheet on narrow viewports |
| **Sheet** | A focused, distinct task that temporarily supersedes the current context | UISheetPresentationController, SwiftUI .sheet | Dialog with focus trap, backdrop, Escape to close |
| **Alert** | Important or unexpected information requiring attention; or critical decision | UIAlertController | Dialog with role="alertdialog", focus trap |
| **Action sheet** | Choices tied to a user-initiated action | UIAlertController action style, SwiftUI confirmationDialog | Bottom sheet on touch, popover on desktop |
| **Confirmation** | A binary or small set of choices that the user must commit to | UIAlertController | Dialog with explicit confirm / cancel |

These are NOT interchangeable. A popover is for contextual supplementary UI. A sheet is for a focused task. An alert is for important information or critical decision. An action sheet is for user-initiated choices. A confirmation is for a binary commit.

## Popover

A popover is anchored to a source element. It is lightweight and dismissable. It adapts to the available space:

- On Desktop Web (wide viewport): popover anchored to the source.
- On Touch Web / narrow viewport: popover becomes a sheet (or a bottom sheet).

The popover must:

- Be related to the source (geometry, anchor, visual continuity).
- Be lightweight (not a multi-step form).
- Dismiss predictably (tap outside, Escape on Web, system Back).
- Return focus to the source on dismiss.

## Sheet

A sheet is for a focused, distinct task that temporarily supersedes the current context. The hierarchy behind the sheet is preserved. Dismissing the sheet does not navigate back.

A sheet is appropriate for:

- A focused, distinct task (edit, compose, review).
- A multi-step form.
- A presentation that requires the user's attention but is part of the workflow.

A sheet is NOT appropriate for:

- Success messages.
- Generic confirmations (use alert or action sheet).
- Supplementary information (use inline disclosure).

A sheet has:

- A presenting source (the element that triggered the sheet).
- A presentation (the sheet itself).
- A detent / size (compact, medium, large).
- Interactive content.
- Dismissal (Done, Cancel, swipe-to-dismiss, tap outside, Escape on Web).
- A commit / cancel semantic.

### Sheet dismissal

A sheet may be dismissed by:

- An explicit Done / Close button.
- An explicit Cancel button.
- A swipe-to-dismiss gesture (iOS 26+ on some sheet types; Web may ADAPT).
- A tap on the backdrop (Web and some iOS sheet types).
- The system Back gesture (iOS — but only if the sheet is part of the navigation stack; in iOS 26, a non-modal sheet may not respond to Back).
- Escape on Web (keyboard).

The Skill records which dismissal variants are appropriate for each sheet type. A sheet that contains unsaved destructive content must require explicit dismissal (Done / Cancel), not interactive dismissal (swipe / backdrop / Escape).

## Alert

An alert is for:

- Important or unexpected information.
- A critical decision that the user must make.

An alert is NOT for:

- Success messages.
- Generic confirmations.
- Multi-step flows.

An alert on Web uses `role="alertdialog"` and a focus trap. The user must explicitly dismiss it.

## Action sheet

An action sheet is for:

- Choices tied to a user-initiated action (the user has done something; the sheet presents the choices that follow).

An action sheet is distinct from an alert:

- Alert: unexpected, important, the user did not initiate the action that caused the alert.
- Action sheet: the user initiated an action; the action sheet presents the choices that follow.

The Skill records this distinction. Apple HIG explicitly advises treating these as separate.

## Confirmation

A confirmation is a small set of choices (often binary: confirm / cancel) that the user must commit to. A destructive action is a common trigger.

A confirmation may be:

- An alert (alertdialog role) for critical / irreversible.
- An action sheet (or popover) for non-critical / reversible.

The Skill records which is appropriate.

## Source continuity

A popover or contextual surface is anchored to its source. The relationship is visible. The user can see the connection between the trigger element and the presented surface.

iOS 26 introduces stronger source-presentation continuity for sheets and popovers. The Skill records this as VERSION-SPECIFIC.

Web approximations of source continuity are heuristic:

- The popover's transform origin is set to the source element.
- The popover's geometry is computed from the source's bounding box.
- On dismiss, the popover animates back toward the source.

The Skill does not recommend a specific implementation; it records the principle.

## Focus return

When an overlay closes, focus returns to:

- The element that triggered the overlay (the trigger element).
- If the trigger element no longer exists, focus moves to the next logical target.

This is a hard invariant. A Skill that does not implement focus return is broken.

## Unsaved-change protection

If an overlay contains unsaved destructive content, dismissal must be protected:

- Explicit confirmation (e.g. "Discard changes?").
- Autosave (save on dismiss).
- Prevent dismissal (the user must explicitly Cancel or Save).

The Skill records the unsaved-change protocol. The implementation is heuristic; the principle is hard.

## Web modal semantics

On Web, modal overlays must:

- Trap focus within the modal (Tab / Shift+Tab cycle within the modal).
- Return focus to the trigger on dismiss.
- Close on Escape (where appropriate).
- Provide a backdrop (visual + interaction).
- Use appropriate ARIA roles (`dialog`, `alertdialog`).
- Provide screen-reader semantics (`aria-modal`, `aria-labelledby`, `aria-describedby`).
- Contain scroll within the modal where appropriate.

The Skill records these as hard invariants for Web.

## URL / modal boundary

Some content that looks like a modal is actually an important independent destination:

- A page with a deep link.
- A page in browser history.
- A long, complex task.
- A primary destination for a user flow.

In these cases, the content should be a navigation destination (URL + back button), not a modal. A modal that is hard to link, hard to bookmark, or contains a long task is a misuse.

## Ownership boundaries

- **Modality decision** (does this need modal at all?) — `apple-modality-overlays`.
- **Presentation type** (sheet / popover / menu / alert / action sheet) — `apple-modality-overlays`.
- **Dismissal semantics** (focus return, unsaved-change protection) — `apple-modality-overlays`.
- **Sheet interactive dismiss gesture** — `apple-direct-manipulation` (cited).
- **Sheet entry / dismiss motion** — `apple-motion-physics` (cited).
- **Source continuity and anchoring** — `apple-modality-overlays`.
- **Selection / focus state** — `apple-control-states`.
- **Web modal focus trap, ARIA** — `apple-modality-overlays` (Web-specific guidance).
- **Press / hover state on overlay trigger** — `apple-control-states`.

## Reference catalog (Tier 2)

- `references/modality-decision-tree.md` — when to use each modality type, with examples.
- `references/sheets.md` — sheet semantics, dismissal, focus return, source continuity.
- `references/popovers-and-menus.md` — popover anchoring, menu semantics, narrow-viewport adaptation.
- `references/alerts-and-confirmation.md` — alert vs confirmation, when to use each.
- `references/focus-and-dismissal.md` — focus return, unsaved-change protection, screen-reader semantics.
- `references/source-continuity.md` — anchoring, source-presentation relationship, iOS 26 source continuity.

## Hard invariants (recap)

1. First question: does this need modality at all? (HARD)
2. System primitive first (HARD).
3. No alert for success messages (HARD).
4. No modal matryoshka (HARD).
5. Focus return on dismiss (HARD).
6. Unsaved-change protection (HARD).
7. iOS 26+ is VERSION-SPECIFIC (HARD).
8. Source continuity (HARD).
9. Web modal focus trap, ARIA, Escape — required (HARD).
10. The 7 stable v1.1 Skills are READ-ONLY.

## Companion files

- `references/modality-decision-tree.md`
- `references/sheets.md`
- `references/popovers-and-menus.md`
- `references/alerts-and-confirmation.md`
- `references/focus-and-dismissal.md`
- `references/source-continuity.md`
