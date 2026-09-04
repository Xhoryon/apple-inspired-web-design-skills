# Command Scope

The Skill's rule: **a command's target is determined by the focused context when platform architecture supports it.** Implementation uses platform primitives (`FocusedValue` on SwiftUI; first-responder chain on AppKit).

## Hard rule

> **A command's target is determined by the focused context when platform architecture supports it.**

A "Save" command in a multi-document editor should target the focused document, not an arbitrary global singleton. A "Duplicate" command in a list should duplicate the focused list item, not the entire selection set when the focus narrows it.

## Per-platform implementation

| Platform | Implementation | Verified API |
|---|---|---|
| SwiftUI | `@FocusedValue` / `FocusedValues` exported from the focused sub-hierarchy; consumed by `Commands` / menu items | DocC FocusedValue / FocusedValues; installed SwiftUI |
| AppKit | First-responder chain; the responder that responds to a menu command is the focused target | DocC NSResponder; installed AppKit NSResponder.h |
| UIKit | UIResponder chain; key commands route through the chain | DocC UIKeyCommand; installed UIKit UIKeyCommand.h |
| Web | Native event delegation; closest `.closest('[data-cmd]')` from event target | HTML Living Standard; WAI-ARIA |

The Skill does NOT prescribe a single algorithm. Each platform uses its native primitive.

## Command enablement

A command should reflect current context:

- "No selection" → Delete Selection command disabled / unavailable.
- "No document open" → Save command disabled / unavailable.
- A shortcut trigger should not invoke an invalid action while the equivalent menu / control says disabled.

The Skill publishes the rule:

> **Command semantics must remain consistent across invocation channels.**

A shortcut trigger for "Save" should produce the same behavior as the menu item "Save" should produce the same behavior as the toolbar button "Save" should produce the same behavior as the programmatic API call.

## Scope leak prevention

The Skill rejects **Command Scope Leak**:

- "Save shortcut always targets the last opened document regardless of focus" → rejected; use the focused document.
- "Delete Selection acts on a hidden / background selection when the focus is elsewhere" → rejected; use the focused item.
- "Duplicate applies to a non-focused pane when the user is in a different pane" → rejected; scope to the focused pane.

The Skill does NOT publish a universal scope algorithm. The app designer wires scope appropriately per platform.

## Layer C command model (Skill synthesis)

```
User Intent
    ↓
Command
    ↓
Current Scope / Focus
    ↓
Availability
    ↓
Shortcut / Menu / Control Entry Point
    ↓
Action
```

- **User Intent** — what the user wants.
- **Command** — the canonical action name (e.g. "Save", "Duplicate", "Delete Selection").
- **Current Scope / Focus** — the focused context that determines the target.
- **Availability** — whether the command is enabled in the current context.
- **Shortcut / Menu / Control Entry Point** — invocation paths.
- **Action** — the actual operation.

## What the Skill owns

- The rule that scope follows focus.
- The rule that enablement is consistent across invocation channels.
- The rule that scope leak is rejected.

## What the Skill does NOT own

- The visual appearance of a menu item — `apple-modality-overlays` / `apple-ui-components`.
- The motion of a command animation — `apple-motion-physics`.
- The save operation itself — `apple-feedback-response` for success / failure semantics.

## Reference

- `references/swiftui-focus-commands.md` — `@FocusedValue` / `FocusedValues` for scope.
- `references/appkit-responder-commands.md` — first-responder chain for scope.
- `references/shortcuts-and-conflicts.md` — scope in shortcut design.