# UIKit Text Input

UIKit text input / editing APIs (verified). Documented in current DocC + installed UIKit.

## Single-line: `UITextField`

`UITextField` (iOS 2.0+ / iPadOS 2.0+ / Mac Catalyst 13.0+ / tvOS 9.0+) provides:

- text input;
- selection (caret + range);
- IME / composition (`markedTextRange`, `setMarkedText(_:selectedRange:)`, `unmarkText`);
- standard edit actions (Cut / Copy / Paste / Select / Select All / Delete);
- clipboard (`UIPasteboard`);
- spell / autocorrect / Writing Tools (when system provides);
- accessibility (when used as a system control);
- secure text entry (`isSecureTextEntry`).

## Multi-line: `UITextView`

`UITextView` (iOS 2.0+ / iPadOS 2.0+ / Mac Catalyst 13.0+ / tvOS 9.0+) provides the same facilities as `UITextField` plus multi-line editing, rich text (when bound to `NSAttributedString`), and text view interactions.

## `UITextInput` protocol

`UITextInput` (iOS 3.2+) defines the text input contract that custom editors adopt:

- `selectedTextRange: UITextRange?` — current selection (caret if empty range, selection if non-empty).
- `markedTextRange: UITextRange?` — IME composition range; `nil` when no composition in progress.
- `setMarkedText(_:selectedRange:)` — receive IME composition update.
- `unmarkText()` — commit / clear composition.
- `textRange(from:to:)` / `position(from:offset:)` — range / position computation.
- `text(in:)` — text in a range.

## `UITextSelectionRect`

`UITextSelectionRect` (iOS 6.0+) is used for multi-range selection (e.g. for accessibility / magnification).

## Modern edit menu: `UIEditMenuInteraction`

`UIEditMenuInteraction` (iOS 16+ / iPadOS 16+ / Mac Catalyst 16+) is the **current default** for edit-menu presentation. Replaces legacy `UIMenuController` for new code.

## Legacy / superseded: `UIMenuController`

`UIMenuController` (iOS 3.2+ / iPadOS 3.2+) is **legacy**. The header still exists in the SDK; current DocC guidance favors `UIEditMenuInteraction` for new code. The Skill does NOT teach `UIMenuController` as the current default.

## `UIResponderStandardEditActions`

`UIResponderStandardEditActions` (iOS 3.0+) provides: `cut(_:)`, `copy(_:)`, `paste(_:)`, `select(_:)`, `selectAll(_:)`, `delete(_:)`, `toggleBoldface(_:)`, `toggleItalics(_:)`, `toggleUnderline(_:)`, etc. The responder chain routes these. Override in your responder to customize.

## `UIPasteboard`

`UIPasteboard` (iOS 3.0+ / iPadOS 3.0+ / Mac Catalyst 13.0+ / tvOS 9.0+) provides system pasteboard with multiple representations (`.string`, `.image`, `.url`, custom UTI).

## Secure text: `isSecureTextEntry`

`UITextField.isSecureTextEntry = true` configures secure entry: typically disables autocorrection, smart quotes, copy (depending on configuration). Standard text editing otherwise applies.

## What the Skill does NOT do

- Does NOT recommend implementing `UITextInput` from scratch unless custom editor is justified.
- Does NOT teach `UIMenuController` as the current default.
- Does NOT assume parity with AppKit / Web text systems.

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/ime-composition.md` — IME composition.
- `references/text-selection.md` — caret / selection.
- `references/selection-continuity.md` — selection continuity.
- `references/edit-actions-and-menus.md` — edit actions.
- `references/clipboard-paste.md` — pasteboard.