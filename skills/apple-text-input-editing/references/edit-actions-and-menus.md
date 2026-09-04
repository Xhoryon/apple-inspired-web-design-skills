# Edit Actions & Menus

Standard edit actions (`cut` / `copy` / `paste` / `select` / `selectAll` / `delete`), edit-menu presentation, and the `UIMenuController` → `UIEditMenuInteraction` transition.

## Standard edit actions

| Action | UIKit (`UIResponderStandardEditActions`) | AppKit (responder chain) | Web (native) |
|---|---|---|---|
| Cut | `cut(_:)` | `cut:` (first responder) | native (`Cmd/Ctrl+X`) |
| Copy | `copy(_:)` | `copy:` | native (`Cmd/Ctrl+C`) |
| Paste | `paste(_:)` | `paste:` | native (`Cmd/Ctrl+V`) |
| Select | `select(_:)` | `select:` | native (programmatic) |
| Select All | `selectAll(_:)` | `selectAll:` | native (`Cmd/Ctrl+A`) |
| Delete | `delete(_:)` | `delete:` | native (`Delete` / `Backspace`) |
| Toggle Boldface | `toggleBoldface(_:)` | `toggleBoldface:` | n/a (web: rich-text editor) |
| Toggle Italics | `toggleItalics(_:)` | `toggleItalics:` | n/a |
| Toggle Underline | `toggleUnderline(_:)` | `toggleUnderline:` | n/a |

System text controls already conform. Override in your responder to customize; the default `UIResponder` implementations may be no-ops for non-editable contexts.

## Availability depends on

- **Current selection** (empty selection + Copy = typically no-op).
- **Editable state** (disabled editor → no edit actions).
- **Clipboard / pasteboard content** (Paste unavailable if pasteboard is empty).
- **Platform** (some actions are UIKit-only / AppKit-only / Web-only).

## Edit-menu presentation

| Platform | Modern mechanism | Legacy / superseded |
|---|---|---|
| iOS / iPadOS | `UIEditMenuInteraction` (iOS 16+) | `UIMenuController` (legacy; superseded) |
| macOS | AppKit menu system (NSMenu / NSMenuItem) | n/a |
| Web | native browser edit menu + custom via `beforeinput` / `input` events | `document.execCommand` (legacy) |

## `UIMenuController` → `UIEditMenuInteraction`

`UIMenuController` is **legacy** in current iOS 16+ context; current DocC guidance favors `UIEditMenuInteraction` for new code. The Skill does NOT teach `UIMenuController` as the current default.

- `UIEditMenuInteraction` (iOS 16+ / iPadOS 16+ / Mac Catalyst 16+) — modern edit-menu presentation.
- Supports touch + pointer + secondary click.
- Adapts to input mode.
- Customizable configuration via `UIEditMenuConfiguration`.

## Edit-menu adapts to input (where supported)

Edit-menu presentation is platform-adapted:
- Touch: long-press → menu appears near the selection.
- Pointer / secondary click: right-click → menu appears at the cursor.
- Keyboard: Esc → menu dismisses; arrow keys navigate.

The Skill does NOT manually replicate this platform adaptation unless necessary.

## Hard principles

- **Standard edit actions preserved.** Cut / Copy / Paste / Select / Select All / Delete provided by system controls. Custom edit menu should add semantic value, not gratuitously replace.
- **Deprecated API default rejected.** `UIMenuController` is not taught as the current default; `UIEditMenuInteraction` is the current default for new code.
- **System primitive first.** Use `UIEditMenuInteraction` (iOS 16+) or AppKit `NSMenu` (macOS); do not custom-build edit menu chrome for fidelity.

## Reference

- `references/clipboard-paste.md` — paste policy.
- `references/uikit-text-input.md` — UIKit text input + edit actions.
- `references/appkit-text-system.md` — AppKit text system.
- `references/web-ime-editing.md` — Web editing.