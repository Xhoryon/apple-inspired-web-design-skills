# Presentation Taxonomy Reference (Orchestrator View)

The presentation taxonomy for Apple-platform overlays. This file is the Tier 2 reference for `apple-experience-design` (the orchestrator). The detailed decision tree is `apple-modality-overlays/references/modality-decision-tree.md`; this file is a summary the orchestrator can cite.

> The presentation taxonomy is owned by `apple-modality-overlays`. This file is a one-page index for the orchestrator; the full content is in the modality-overlays references.

## The taxonomy

| Type | When | System primitive (Native Apple) | Web pattern | Detailed reference |
|---|---|---|---|---|
| **Menu / context menu** | Commands / actions on the current selection or surface | `UIContextMenuInteraction`, `NSMenu` | Right-click (mouse), long-press (touch), keyboard shortcut | `apple-modality-overlays/references/popovers-and-menus.md` |
| **Popover** | Contextual supplementary UI anchored to a source | `UIPopoverPresentationController` | Anchored element + ARIA; ADAPT to sheet on narrow viewports | `apple-modality-overlays/references/popovers-and-menus.md` |
| **Sheet** | A focused, distinct task that temporarily supersedes the current context | `UISheetPresentationController`, SwiftUI `.sheet` | Dialog with focus trap, backdrop, Escape to close | `apple-modality-overlays/references/sheets.md` |
| **Alert** | Important or unexpected information requiring attention; or critical decision | `UIAlertController` | Dialog with `role="alertdialog"`, focus trap | `apple-modality-overlays/references/alerts-and-confirmation.md` |
| **Action sheet** | Choices tied to a user-initiated action | `UIAlertController` action style, SwiftUI `confirmationDialog` | Bottom sheet on touch, popover on desktop | `apple-modality-overlays/references/alerts-and-confirmation.md` |
| **Confirmation** | A binary or small set of choices that the user must commit to | `UIAlertController` | Dialog with explicit confirm / cancel | `apple-modality-overlays/references/alerts-and-confirmation.md` |

These are NOT interchangeable. A popover is for contextual supplementary UI. A sheet is for a focused task. An alert is for important information or critical decision. An action sheet is for user-initiated choices. A confirmation is for a binary commit.

## Decision shortcut

The full decision tree is `apple-modality-overlays/references/modality-decision-tree.md`. The shortcut:

1. Is this content supplementary? → inline disclosure, NOT a modal.
2. Is this a focused distinct task? → sheet.
3. Is this a contextual action on a specific element? → popover (Desktop Web) or action sheet (touch).
4. Is this a destructive action the user might not intend? → confirmation (alert for critical, action sheet for non-critical).
5. Is this a command menu? → menu / context menu.
6. Is this unexpected information? → alert.

## Cross-Skill note

- The decision tree is `apple-modality-overlays/references/modality-decision-tree.md`.
- The sheet semantics, dismissal, focus return are `apple-modality-overlays/references/sheets.md`.
- The popover / menu distinction is `apple-modality-overlays/references/popovers-and-menus.md`.
- The alert / confirmation distinction is `apple-modality-overlays/references/alerts-and-confirmation.md`.
- The focus return and unsaved-change protection are `apple-modality-overlays/references/focus-and-dismissal.md`.
- The source continuity is `apple-modality-overlays/references/source-continuity.md`.
- The ownership of the presentation decision is `apple-modality-overlays` (not the orchestrator).
