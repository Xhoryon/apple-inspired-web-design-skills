# Focus Topology

The Skill-owned focus topology model. This is **Apple Experience Skill implementation vocabulary**, NOT extracted from Apple HIG as a literal primitive set.

## The topology model (Skill-owned Layer C)

```
Current Focus
      ↓
Focus Scope
      ↓
Reachable Items / Groups
      ↓
Movement Intent
      ↓
Next Valid Target
      ↓
Activation / Command Context
```

Each layer is an answer to a separate question:

- **Current Focus** — which element currently receives keyboard input.
- **Focus Scope** — which context the focus lives in (field group; list; sidebar; pane; modal; window; scene).
- **Reachable Items / Groups** — what items are reachable from this scope (Tab order; arrow-key grid; Full Keyboard Access cross-group).
- **Movement Intent** — what the user wants to do (next field; previous; into sub-tree; exit sub-tree; modal close).
- **Next Valid Target** — the next element after applying movement intent.
- **Activation / Command Context** — what activation / command fires when the focused element triggers an action.

## Per-platform scope examples

| Platform | Typical scopes | Movement primitives |
|---|---|---|
| iOS / iPadOS (with keyboard) | field group; list; modal; scene | Tab; arrow keys; hardware-keyboard navigation; accessibility |
| iOS / iPadOS (touch only) | modal; scene | native touch; on-screen keyboard for text fields |
| macOS | field group; list; sidebar; content pane; inspector; modal; window; application | Tab; arrow keys; ⌘ shortcuts; menu bar; Full Keyboard Access |
| tvOS | focus-driven (entire interaction model) | remote / focus engine |
| visionOS | spatial UI (focus + gaze + pinch) | gaze + pinch; spatial focus |
| watchOS | stack-driven; Digital Crown | Digital Crown; small focus list |
| Web (Desktop / Touch) | page; dialog; field group; component | Tab; Shift-Tab; arrow keys; native focus order |

## Movement intent → behavior

| Movement intent | Behavior |
|---|---|
| Next focusable | Tab (Web); platform equivalent (Apple) |
| Previous focusable | Shift-Tab (Web); platform equivalent (Apple) |
| Into a sub-tree | arrow key (where supported); platform equivalent |
| Exit a sub-tree | platform escape (e.g. macOS: Escape; or navigate to parent) |
| Cross-group | Tab again; arrow keys (Full Keyboard Access); platform equivalent |
| Modal containment | focus stays within modal until dismissal |
| Modal close | focus returns to invoking control or sensible continuation |

## Ownership boundary

The Skill owns:

- **Where** focus moves.
- **How** movement intent is computed.
- **What** scope a focus lives in.
- **Restoration** after dismissal / reflow / deletion.

The Skill does NOT own:

- The visual focus indicator — owned by `apple-control-states`.
- The motion of the focus transition — owned by `apple-motion-physics`.
- The destination semantics (where the user navigated) — owned by `apple-navigation-spatial`.
- The modal type — owned by `apple-modality-overlays`.

## Per-platform focus API (verified)

- **SwiftUI**: `@FocusState`; `.focused(_:)`; `@FocusedValue` / `FocusedValues` / `FocusedBinding` / `.focusedSceneValue(_:)`.
- **UIKit**: `UIFocusSystem`; `UIFocusEnvironment`; `UIFocusItem`; `UIFocusUpdateContext`; `preferredFocusEnvironments` (iOS 11+; NOT native macOS; per installed headers).
- **AppKit**: `NSResponder.firstResponder`; `NSWindow.makeFirstResponder` (macOS only).
- **Web**: native focus order via DOM / semantic structure; `<button>` / `<a>` / `<input>` / `<select>` / `<textarea>` natively focusable; `:focus-visible`; `<dialog>` with `showModal()` for modal focus containment.

## What the Skill rejects

- **Focus Kidnapping** — moving focus on background refresh, async completion, carousel auto-advance, or any state change that did not require it.
- **Focus-Selection Collapse** — auto-selecting on focus traversal when the platform / task does not call for it.
- **Reflow Focus Reset** — resetting focus to top / root on adaptive layout change when task continuity is preserved.
- **Hidden Focus** — suppressing the visible focus indicator for cleaner visuals. WCAG 2.2 SC 2.4.7 violation.
- **Focus = Hover** — making keyboard focus depend on pointer hover.

## Per-platform availability references

See `references/swiftui-focus-commands.md`, `references/uikit-keyboard-focus.md`, `references/appkit-responder-commands.md`, `references/web-keyboard-focus.md`.