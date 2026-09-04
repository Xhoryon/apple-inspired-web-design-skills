# Undo & Recovery

The Skill owns recovery semantics: how undo / redo / restore / undo-presentation / action naming / grouping / relationship with destructive actions. The Skill does NOT decide the modal type for undo presentation — `apple-modality-overlays` does.

## Core model

| Concept | Meaning | Apple surface |
|---|---|---|
| **Undo** | reverse the most recent user action (or a group of actions) | `UndoManager` (Foundation); `NSUndoManager` (AppKit, macOS); iOS shake-to-undo gesture (UIKit, iOS 9+); SwiftUI `@Environment(\.undoManager)` |
| **Redo** | re-apply a previously undone action | same surfaces |
| **Restore** | return content / state to a prior state | undo system; explicit "Restore" command; OS file restoration (NSFileVersion, etc.) |
| **Recovery** | continue after a failure (retry / fallback / manual fix) | retry buttons; error recovery surfaces; undo for the failing operation |

The Skill owns the **semantics** of recovery. The Skill does NOT mandate a specific presentation (snackbar / toast / menu / sheet).

## Why undo is system capability, not snackbar dogma

The skill rejects the rule "every delete must show an Undo toast" as a canonical Apple principle. Apple platforms provide multiple undo affordances:

| Platform | | Affordance |
|---|---|---|
| iOS / iPadOS | shake-to-undo gesture; menu-bar Edit > Undo (macOS-style on iPad with Magic Keyboard); undo via SwiftUI `@Environment(\.undoManager)` |
| macOS | Edit > Undo / Redo menu items; menu shortcuts (⌘Z / ⇧⌘Z); NSUndoManager integration; toolbar undo buttons |
| watchOS | limited undo via WKInterfaceDevice + UndoManager; Digital Crown as scroll / navigate |
| visionOS | spatial undo via window-level menus; UndoManager environment |
| Web | `history.back()` / `history.forward()`; SPA state restoration; application-level undo system |

The Skill chooses the right affordance for the platform, content, and product. It does NOT prescribe "every delete must show an Undo toast".

## Predictable undo result

The Skill publishes the rule: undo must produce a predictable, visible result. The user must understand what happened and be able to verify the action was reversed.

- **Visible**: the reversed content should be where the user can see it (or where they can navigate to it).
- **Understandable**: the undo UI (if any) should clearly indicate what was undone.
- **Repeatable**: the user can undo multiple actions in order; each undo reverses one logical group.
- **Reversible**: redo where appropriate.

## Action naming

The Skill recommends that undo actions be named with a clear, user-facing description:

- **Bad**: "Undo" (no context).
- **Better**: "Undo Delete Photo" / "Undo Move to Trash".

The undo system records the action name; the undo menu / snackbar / toast shows the name.

## Action grouping

The Skill recommends grouping related actions:

- A user typing into multiple text fields in a row → group as "Undo Typing".
- A user deleting three items in a row → group as "Undo Delete 3 Items" if the deletions were a single logical operation; or three separate "Undo Delete Item" if independent.

The grouping is owned by the undo system; the Skill publishes the rule.

## Recoverability vs confirmation

The Skill publishes the relationship between recoverability and confirmation:

| Action type | Recoverable? | Confirmation? |
|---|---|---|
| Routine recoverable (toggle, type, select) | yes (undo available) | NO confirmation needed |
| Significant recoverable (delete with undo) | yes (undo available) | NO confirmation needed (the user can undo) |
| Significant recoverable (financial / security) | yes (undo available) | optional confirmation; user benefits from confidence |
| Irreversible low consequence (clear recent files) | no | optional confirmation |
| Irreversible significant (delete account, send message) | no | confirmation may be appropriate |

The Skill rejects "confirm every delete" as a canonical rule. The Skill rejects "no undo ever" as a canonical rule.

## Undo visibility

When an undo result affects offscreen or non-obvious content:

- Consider showing the result briefly (highlight, scroll-to, brief notification).
- Don't let users repeatedly undo because they think nothing happened.

Examples:

- User deletes an item in a list. The item disappears from the visible list. Undo is offered. If accepted, the item reappears in its previous position. **Visible result.**
- User changes a setting that takes effect on a different surface. Undo is offered. If accepted, the setting reverts. **User may not see the change immediately.** Consider surfacing "Setting reverted" briefly.

## Apple platform surfaces

| Platform | Undo affordance | Redo affordance |
|---|---|---|
| iOS / iPadOS (touch) | shake-to-undo (iOS 9+); Edit > Undo menu (iPad with keyboard) | Edit > Redo menu (iPad with keyboard) |
| iOS / iPadOS (SwiftUI) | `@Environment(\.undoManager)` + custom UI | same |
| macOS | Edit > Undo menu; ⌘Z; toolbar undo button; NSUndoManager | Edit > Redo menu; ⇧⌘Z; NSUndoManager |
| watchOS | `UndoManager` environment (limited UI) | same |
| visionOS | `UndoManager` environment; window-level menus | same |
| Web | `history.back()`; SPA state restoration; application-level undo | `history.forward()`; SPA state restoration; redo command |

## What the Skill rejects

- **Every delete must show an Undo toast** — NOT canonical. Rejected as a rule.
- **No undo ever** — also rejected; many actions benefit from undo.
- **Invisible Undo** — undoing offscreen content without showing what changed. The result must be visible or understandable.
- **Unnamed undo actions** — "Undo" without context. The action name should be clear.
- **Ungrouped rapid actions** — many small actions undoing individually when they were a logical group.
- **Confirmation without undo for routine actions** — the Skill rejects "confirm every delete".
- **Confirmation without undo for irreversible actions** — also rejected; confirmation without undo is friction.