# UIKit Keyboard & Focus

UIKit key commands and focus APIs (verified in DocC + installed SDK). Only after per-symbol verification; the Skill does NOT invent UIKit focus APIs.

## UIKeyCommand

`UIKeyCommand` (DocC `UIKeyCommand`; installed UIKit `UIKeyCommand.h`). Attaches a key command to a responder. UIKit routes the keystroke through the responder chain to a handler.

| Availability | |
|---|---|
| iOS | 7.0+ |
| iPadOS | 7.0+ |
| Mac Catalyst | 13.0+ |
| tvOS | 9.0+ |

`UIKeyCommand` is NOT available on native macOS (AppKit uses `NSResponder` + `NSMenuItem.keyEquivalent` instead).

## UIKeyModifierFlags

`UIKeyModifierFlags` (DocC; installed UIKit). Modifier flag set for `UIKeyCommand`. Includes `.command`, `.shift`, `.alternate` (Option), `.control`, `.numericPad`, etc.

## UIFocusSystem

`UIFocusSystem` (DocC `UIFocusSystem`; installed UIKit `UIFocus.h`). The UIKit focus API for keyboard / pointer / game controller focus management within an app.

| Availability | |
|---|---|
| iOS | 11.0+ |
| iPadOS | 11.0+ |
| Mac Catalyst | 13.0+ |
| tvOS | 11.0+ |

`UIFocusSystem` is NOT available on native macOS (AppKit uses first-responder + key-view loop).

## UIFocusEnvironment / UIFocusItem / UIFocusUpdateContext

- `UIFocusEnvironment` — protocol for any object that can contain focused items.
- `UIFocusItem` — protocol for a focusable item.
- `UIFocusUpdateContext` — context provided during focus updates.

All available iOS 11+ / iPadOS 11+ / Mac Catalyst 13.0+ / tvOS 11+ (NOT native macOS).

## preferredFocusEnvironments

`preferredFocusEnvironments` (DocC). The view declares its preferred focus destinations when focus moves into the view.

| Availability | |
|---|---|
| iOS | 11.0+ |
| iPadOS | 11.0+ |
| Mac Catalyst | 13.0+ |
| tvOS | 11.0+ |

**Note**: the older `preferredFocusedView` API was deprecated in favor of `preferredFocusEnvironments`. The Skill does NOT teach deprecated API as preferred current guidance without explicit legacy context.

## System command priority

UIKit documentation indicates that system handling gets priority for known system events before app key commands. The Skill publishes the rule:

> **Do NOT fight standard system command handling without strong need.**

The Skill does NOT recommend APIs that override system behavior without version-specific verification.

## Responder chain

UIKit routes `UIKeyCommand` instances through the responder chain. A view declares `UIKeyCommand` instances; UIKit dispatches the keystroke to the first responder and walks up the chain.

The Skill does NOT invent a key-command manager class. UIKit uses the responder chain; the app wires `UIKeyCommand` instances to responders.

## What the Skill does NOT do

- Does NOT cite `UIFocusEngine` (fabricated; private; the public API is `UIFocusSystem`).
- Does NOT cite `UIKeyboardCommandManager` (fabricated; not in installed UIKit).
- Does NOT cite `UIFocusCommandManager` (fabricated; not in installed UIKit).
- Does NOT cite `UIFocusGroup` as a standalone class (`UIFocusGroup` does NOT exist; focus groups are conceptual via `UIFocusEnvironment` hierarchy).
- Does NOT recommend UIKit key commands on native macOS (use AppKit instead).

## Reference

- `references/focus-topology.md` — focus topology.
- `references/focus-continuity.md` — restoration.
- `references/keyboard-navigation.md` — traversal.
- `references/shortcuts-and-conflicts.md` — shortcut scope + conflicts.