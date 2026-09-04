# Clipboard & Paste

Clipboard / pasteboard semantics. The Skill's rule: **paste is destination-driven; clipboard content may have multiple representations.**

## Clipboard / pasteboard basics

| Platform | Type | Multiple representations |
|---|---|---|
| iOS / iPadOS / Mac Catalyst | `UIPasteboard` | yes (`.string`, `.image`, `.url`, custom) |
| macOS | `NSPasteboard` | yes (multiple types) |
| Web | Clipboard API (with permission model) | yes (text/plain, text/html, image/png, custom) |

## Paste policy

The Skill publishes the rule: **paste is destination-driven.**

| Destination | Policy |
|---|---|
| Plain text field / editor | strip rich formatting; paste as plain text |
| Rich text field / editor | preserve formatting; paste with attributes |
| Code editor / structured editor | paste as plain text; preserve semantic structure if recognized |
| Secure field | restrict copy / paste per platform secure-text configuration |

The Skill does NOT make one universal Apple rule without evidence. Product / editor semantics matter.

## Paste is not string-only

**Reject the anti-pattern:** "clipboard content is always plain string."

Clipboard / pasteboard content may have multiple representations:
- `text/plain` (UTF-8 string)
- `text/html` (formatted text)
- `image/png` (image)
- `application/json` (structured data)
- custom UTI / MIME type

The editor chooses how to interpret the paste. Simple text fields may intentionally request plain text.

## Multiple representations (pasteboard types)

- iOS: `UIPasteboard.general` exposes the system pasteboard; check `.hasStrings`, `.hasImages`, etc.
- macOS: `NSPasteboard.general` exposes the system pasteboard; query `types` to see available representations.
- Web: `navigator.clipboard.read()` (with permission); `DataTransfer.types` on paste event.

## Paste + composition

When pasting into a field with active IME composition:
- The composition is interrupted.
- The pasted text becomes the new value (subject to paste policy).
- The caret is positioned at the end of the pasted text.

The Skill does NOT combine paste with ongoing composition in a custom way.

## Reference

- `references/edit-actions-and-menus.md` — Paste action.
- `references/plain-rich-text.md` — plain vs rich text.
- `references/uikit-text-input.md` — `UIPasteboard`.
- `references/appkit-text-system.md` — `NSPasteboard`.
- `references/web-ime-editing.md` — Web Clipboard API.