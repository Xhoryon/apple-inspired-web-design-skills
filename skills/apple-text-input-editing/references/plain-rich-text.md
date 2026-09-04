# Plain vs Rich Text

The Skill's distinction between plain text and rich text. These are Skill-defined categories; they are not verbatim Apple HIG terms.

## Plain text

- Content semantics: characters + line breaks.
- No styling, attributes, or attachments.
- Examples: chat messages, code, log entries, search input.
- Typical SwiftUI / UIKit / AppKit surfaces: `TextField`, `TextEditor` (without attributed binding), `UITextField` (non-attributed), `NSTextField` (non-attributed), Web `<input>` / `<textarea>`.

## Rich text

- Content semantics: characters + styling (font, color, weight, size) + attributes (links, attachments, lists, tables).
- Model: `NSAttributedString` on Apple platforms; HTML on Web.
- Examples: rich notes, formatted emails, word-processor documents.
- Typical surfaces: `UITextView` (with attributed binding), `NSTextView`, `contenteditable` on Web, `NSTextField` with attributed text.

## Per-platform data model

| Platform | Plain text type | Rich text type |
|---|---|---|
| UIKit | `String` | `NSAttributedString` |
| AppKit | `String` | `NSAttributedString` |
| SwiftUI | `String` (default) / `Binding<String>` | `AttributedString` / `Binding<AttributedString>` |
| Web | string (textContent / value) | HTML (innerHTML) — for `contenteditable` |

## Editor selection

- Plain text editor: caret + range; no inline styling semantics.
- Rich text editor: caret + range + style attributes (font, color, weight, etc.) of the selected range.

## Paste policy recap

- Plain text editor: paste as plain text (strip rich formatting).
- Rich text editor: paste with attributes (preserve formatting).

## Custom editor gate

Custom rich-text editor is justified for:
- specialized document models (notes, blog editor, email composer, word processor);
- advanced layout (canvas, mixed content types);
- domain-specific structured content.

Comes with IME / accessibility / clipboard / editing commands / Writing Tools / multi-range selection responsibilities.

## Hard principle

> **Use system text controls (UITextField / UITextView / NSTextField / NSTextView / SwiftUI TextField / TextEditor / Web `<input>` / `<textarea>` / `contenteditable`) before custom text engine.**

Reject "build a custom text engine for visual novelty."

## Reference

- `references/clipboard-paste.md` — paste policy.
- `references/edit-actions-and-menus.md` — edit actions.
- `references/uikit-text-input.md` — UIKit.
- `references/appkit-text-system.md` — AppKit.
- `references/web-ime-editing.md` — Web `contenteditable`.