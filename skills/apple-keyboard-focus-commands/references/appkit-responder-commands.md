# AppKit Responder & Commands

AppKit responder-chain + menu + command APIs (verified in DocC + installed SDK). macOS-only native.

## NSResponder

`NSResponder` (DocC; installed AppKit `NSResponder.h`). Base class for the responder chain. `NSResponder.firstResponder` is the current keyboard input target.

| Availability | |
|---|---|
| macOS | 10.0+ |

## NSWindow.firstResponder / NSWindow.makeFirstResponder

`NSWindow.firstResponder` / `makeFirstResponder` (DocC `NSWindow`; installed AppKit `NSWindow.h`). Manage the window-level keyboard focus.

| Availability | |
|---|---|
| macOS | 10.0+ |

The Skill does NOT generalize `NSResponder`-style first-responder into "all Apple platforms use first-responder". UIKit / SwiftUI conceptual parallels do NOT imply API identity.

## NSMenu / NSMenuItem

`NSMenu` (DocC; installed AppKit). `NSMenuItem` (DocC; installed AppKit `NSMenuItem.h`). AppKit menu system.

| Availability | |
|---|---|
| macOS | 10.0+ |

`NSMenuItem.keyEquivalent` (Swift: `keyEquivalent`) declares the keyboard shortcut for a menu item.

```swift
NSMenuItem(title: "Save", action: #selector(save(_:)), keyEquivalent: "s")
NSMenuItem(title: "Save", action: #selector(save(_:)), keyEquivalent: KeyEquivalent("s"), modifiers: [.command])
```

## First responder

`firstResponder` is fundamental to AppKit command / event routing. The first responder is the recipient of keyboard events and menu commands. The responder chain propagates upward if the first responder does not handle the event.

## Key view loop

AppKit windows have a key view loop: the first responder is one of the key views. The view loop is used for keyboard navigation and command targeting.

## Menu validation

AppKit menu items can be validated: a target's `validateMenuItem(_:)` returns whether the menu item should be enabled. The Skill publishes the rule:

> **Command semantics must remain consistent across invocation channels.**

A menu item that is disabled should also be unreachable via shortcut. A shortcut that triggers an action should also have a corresponding enabled menu item (when a menu is available).

## macOS menu bar

macOS uses the menu bar as a core command surface. Important commands should be accessible through the menu bar.

The Skill publishes the rule:

> **Do NOT hide every useful command behind context menu / toolbar icon / shortcut-only path.**

The menu bar is a primary command surface on macOS. Hiding important commands violates platform conventions.

## What the Skill owns

- The rule that scope follows focus (focused context).
- The rule that shortcuts respect standard platform semantics.
- The rule that menu items are validated consistently.

## What the Skill does NOT own

- Menu visual design (`apple-modality-overlays` / `apple-ui-components`).
- Component appearance (`apple-ui-components`).

## What the Skill rejects

- **Menu-less Mac** — keeping all commands in toolbar icons; menu bar is required for important commands.
- **Shortcut-only Mac** — shortcut as the only path to important commands; menu items with shortcut hints should also exist.

## Reference

- `references/command-scope.md` — scope rule + enablement consistency.
- `references/shortcuts-and-conflicts.md` — shortcut scope + standard preservation.
- `references/keyboard-navigation.md` — traversal.
- `references/keyboard-accessibility.md` — accessibility.