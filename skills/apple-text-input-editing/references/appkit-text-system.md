# AppKit Text System

AppKit text input / editing APIs (verified). Documented in current DocC + installed AppKit.

## Single-line / wrapped: `NSTextField`

`NSTextField` (macOS 10.0+) provides:

- single-line or wrapped editable text;
- IME / composition (when using `NSTextField` with editable behavior);
- standard edit actions via responder chain;
- pasteboard (`NSPasteboard`);
- accessibility (when using system control);
- secure variant: `NSSecureTextField` (macOS 10.0+).

## Multi-line: `NSTextView`

`NSTextView` (macOS 10.0+) provides:

- multi-line editable text;
- multi-range selection (via `selectedRanges`);
- IME / composition via `NSTextInputClient`;
- undo / redo (via `UndoManager` integration);
- pasteboard;
- rich text via `NSTextStorage` / `NSAttributedString`;
- key bindings;
- spell / autocorrect / smart quotes / smart links / Writing Tools (when system provides);
- services;
- accessibility.

Modern `NSTextView` uses TextKit 2 (`NSTextLayoutManager` + `NSTextContentManager` + `NSTextContentStorage`).

## Modern: `NSTextInputClient`

`NSTextInputClient` (macOS 10.0+) is the **current contract** for text input on AppKit. Replaces legacy `NSTextInput` (deprecated / superseded).

## Legacy / superseded: `NSTextInput`

`NSTextInput` (macOS 10.0+) is **legacy**. The Skill documents `NSTextInputClient` as the modern replacement. The Skill does NOT teach `NSTextInput` as the current default.

## `NSTextLayoutManager` (TextKit 2)

`NSTextLayoutManager` (macOS 10.15+ / iOS 15.0+) is the modern TextKit 2 layout manager. Use only when custom layout is required; the default text system handles most cases.

## `NSAttributedString`

`NSAttributedString` (macOS 10.0+ / iOS 3.2+ / etc.) is the rich-text data model on Apple platforms. Attributes include font, color, paragraph style, link, attachment.

## Edit actions

Edit actions on macOS are routed through the responder chain. The first responder can override `cut:`, `copy:`, `paste:`, `selectAll:`, `delete:`, `toggleBoldface:`, `toggleItalics:`, `toggleUnderline:` etc.

`NSMenu` / `NSMenuItem` participate in the menu bar; `NSMenuItem.keyEquivalent` declares the keyboard shortcut. Menu validation enables / disables items based on the responder context.

## `NSPasteboard`

`NSPasteboard` (macOS 10.0+) provides the system pasteboard. Multiple representations: `string` (UTF-8), `RTF` (`NSAttributedString`), images, custom UTI.

## What the Skill does NOT do

- Does NOT recommend implementing `NSTextInputClient` from scratch unless custom editor is justified.
- Does NOT teach `NSTextInput` as the current default.
- Does NOT assume parity with UIKit / Web text systems.

## Reference

- `references/text-input-lifecycle.md` — input lifecycle.
- `references/ime-composition.md` — IME composition.
- `references/text-selection.md` — caret / selection (multi-range).
- `references/selection-continuity.md` — selection continuity.
- `references/edit-actions-and-menus.md` — edit actions.
- `references/clipboard-paste.md` — pasteboard.