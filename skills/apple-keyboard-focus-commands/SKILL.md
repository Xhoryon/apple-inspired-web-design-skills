---
name: apple-keyboard-focus-commands
description: Use when designing how focus moves and persists within an app, and how keyboard / command input targets and activates actions within the current task context. Owns focus topology, focus movement, focus restoration, keyboard traversal, command semantics, keyboard shortcuts, default / cancel actions, command scope, command discoverability, menu-command relationships, and the keyboard alternative path for essential gestures. Applies to Native iOS / iPadOS / macOS / Mac Catalyst / visionOS / tvOS / watchOS and Web (Desktop / Touch / PWA). Do NOT use for the visual appearance of a focused control (use apple-control-states), navigation destination / history semantics (use apple-navigation-spatial), motion used during focus transition (use apple-motion-physics), modal choice / presentation (use apple-modality-overlays), structural reflow (use apple-adaptive-structure), drag mechanics (use apple-direct-manipulation), success / error meaning (use apple-feedback-response), or component appearance (use stable apple-ui-components).
version: "0.6.0-dev"
license: MIT
---

# apple-keyboard-focus-commands

Keyboard, Focus & Command Interaction. Owns how focus moves and persists within an app, and how keyboard / command input targets and activates actions within the current task context. Does NOT own the visual appearance of a focused control or the motion of the focus transition.

## When to use

Use when the task involves focus traversal, keyboard shortcuts, focus restoration after dismissal / reflow / navigation, command scope, default / cancel actions, or keyboard alternatives to essential gestures. Symptoms: "Tab through this settings panel", "where does focus go after the sheet closes", "command-S should save the focused document", "keyboard alternative for the drag-to-reorder", "the sidebar reflow lost focus", "the shortcut conflicts with the browser".

Do NOT use for: visual focus indicator design (`apple-control-states`); navigation destination semantics (`apple-navigation-spatial`); focus transition motion (`apple-motion-physics`); modal type decision (`apple-modality-overlays`); structural reflow (`apple-adaptive-structure`); drag mechanics (`apple-direct-manipulation`); success / error meaning (`apple-feedback-response`); component appearance (stable v1.1 `apple-ui-components`).

## Core model (Skill-owned vocabulary)

The Skill uses a **focus topology model**. This is **Apple Experience Skill implementation vocabulary**, NOT extracted from Apple HIG as a literal primitive set.

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

The Skill also uses a **command model** (Skill-owned):

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

**Scope rule (Skill synthesis):** a command's target is determined by the focused context (focused document; focused pane; focused item) when platform architecture supports it. Implementation: SwiftUI `FocusedValue` / `@FocusedValue`; AppKit first-responder chain.

## Hard invariants

1. **Focus ≠ Selection.** A focused item may not be selected; a selected item may remain selected while focus moves elsewhere. Do not collapse.
2. **Focus ≠ Hover.** Keyboard focus must not depend on pointer hover. A pointer leaving an element must not erase meaningful keyboard focus. Preserved from v0.1+.
3. **FocusState ≠ FocusedValue** (SwiftUI). `FocusState` is for focus placement; `FocusedValue` is for command context. They serve different roles; do not collapse.
4. **Command ≠ Shortcut.** A command is an action / intent; a shortcut is one invocation path. Important commands are accessible through menu / toolbar / context menu / button / shortcut. Shortcut-only important functionality is rejected.
5. **System primitive first.** Use SwiftUI `FocusState` / `FocusedValue` / `keyboardShortcut(_:)` / `Commands`; UIKit `UIFocusSystem` / `UIFocusEnvironment` / `UIKeyCommand`; AppKit `NSResponder` / `NSMenu` / `NSMenuItem.keyEquivalent`; Web native focus / `<dialog>` / WAI-ARIA. Do NOT reinvent.
6. **No focus stealing** by default. Programmatic focus only in narrow contexts: initial focus; validation; modal restoration; explicit user-triggered continuation.
7. **Focus restoration over arbitrary continuity.** If the focused control survives reflow / dismissal / navigation, focus stays. If it disappears, move to a meaningful reachable target. Do NOT reset to the top / root.
8. **Full Keyboard Access boundary.** Full Keyboard Access is the macOS system capability that provides Tab navigation across the system UI and the app. App-defined focus navigation is what the app builds into its controls. They are distinct; the Skill does NOT teach developers to manually recreate Full Keyboard Access behavior.
9. **Per-platform availability respected.** `UIFocusSystem` is iOS-family; AppKit first-responder is macOS; tvOS is focus-driven; visionOS is focus-driven via spatial primitives; watchOS is not first-class for focus; Web is WAI-ARIA + HTML. The Skill does NOT assume cross-framework or cross-platform parity.
10. **Standard shortcuts respected.** Do NOT casually repurpose ⌘C / ⌘V / ⌘X / ⌘A / ⌘Z / ⇧⌘Z / ⌘S / ⌘Q / ⌘W / ⌘N for unrelated actions. Justification required.
11. **Default / Cancel semantics** preserved. Default = primary contextual action; Cancel = cancel in-progress temporary action or dismiss. Do NOT collapse Escape with Back / Close / Dismiss.
12. **Command scope follows focused context.** When the focused context can supply a target (focused document; focused pane; focused list item), the command targets that. Implementation uses platform primitives.
13. **No keyboard trap.** Non-modal regions must not accidentally imprison focus. Modal focus containment is allowed; dismissal must restore appropriate focus.
14. **Keyboard alternative when essential.** Essential pointer-only gestures (reorder; delete; open; move; zoom) should have a keyboard / command path where keyboard is an applicable alternative. Coordination with `apple-direct-manipulation`.
15. **Three evidence layers explicit.** Layer A (Apple official) / Layer B (Skill synthesis) / Layer C (Skill implementation vocabulary) — never collapsed.
16. **Paraphrase by default.** Apple HIG portal pages return JS-rendered SPA shells; verbatim HIG text is UNVERIFIED in this session. Verbatim HIG text is UNVERIFIED. Paraphrase + derivation.
17. **No fabricated Apple APIs.** Every symbol passes the scanner's `KEYBOARD_FOCUS_COMMANDS` allowlist. No `UIFocusEngine`, `UIKeyboardCommandManager`, `UIFocusCommandManager`, `UIFocusGroup`, etc.

## Routing integration

The orchestrator (`apple-experience-design`) adds a row for the new Skill. **Selective load rule:** the row applies ONLY when the task explicitly involves focus movement / restoration, keyboard shortcuts, command targeting, or keyboard alternatives. For ordinary state / motion / modal / structural / material tasks, the orchestrator routes WITHOUT loading `apple-keyboard-focus-commands`.

Discriminator checklist (the orchestrator uses these to gate loading):

- Is the task about how focus moves within the app? If yes, load.
- Is the task about a keyboard shortcut for a known action? If yes, load.
- Is the task about focus restoration after dismissal / reflow / navigation? If yes, load.
- Is the task about command targeting via focused context? If yes, load.
- Is the task about the keyboard alternative for an essential gesture? If yes, load (often with `apple-direct-manipulation`).
- Is the task purely about a control's visual state? If yes, do NOT load — use `apple-control-states`.
- Is the task about modal type decision? If yes, do NOT load for modal choice; `apple-keyboard-focus-commands` may be conditional for post-dismiss focus continuity.
- Is the task purely about transition motion? If yes, do NOT load — use `apple-motion-physics`.
- Is the task about structural reflow? If yes, do NOT load; `apple-keyboard-focus-commands` may be conditional for focus continuity through reflow.
- Is the task about component appearance? If yes, do NOT load — use stable v1.1 `apple-ui-components`.

## Focus topology (Layer C Skill synthesis)

The Skill models focus as a layered topology:

| Scope | Examples |
|---|---|
| Field group | form section, settings panel |
| List / grid | selectable items in a list / table |
| Sidebar | primary navigation surface |
| Content pane | the residual area after sidebar / inspector |
| Inspector | on-demand right surface |
| Modal | sheet / popover / alert |
| Window / scene | window-level (macOS); scene-level (iOS) |
| Application command context | menu bar / command palette / shortcut scope |

Exact platform behavior varies per platform. Do NOT pretend the same focus-scope API exists everywhere.

## Programmatic focus (Layer B synthesis)

Appropriate in narrow contexts:

- Initial task context: first focusable element of a new screen.
- Validation: focus moves to the specific field that needs attention (with explanation; with accessibility; not repeatedly).
- Previously focused content disappears during explicit directional navigation.
- Modal dismissal: restore focus to the invoking control or a meaningful continuation.
- User-triggered structural action: predictable continuation target.

NOT for: state-change decoration; background refresh ticks; carousel auto-advance; async completion that did not need focus. Focus stealing is the Focus Kidnapping anti-pattern.

## Reference catalog (Tier 2)

- `references/focus-topology.md` — focus topology + scope rule + movement patterns.
- `references/focus-continuity.md` — focus restoration after modal / reflow / navigation / deletion.
- `references/keyboard-navigation.md` — Tab / Shift-Tab / directional traversal / group traversal / focus order.
- `references/shortcuts-and-conflicts.md` — shortcut scope, standard shortcut preservation, conflict resolution.
- `references/command-scope.md` — scope rule; FocusedValue / first-responder wiring; enablement consistency.
- `references/swiftui-focus-commands.md` — SwiftUI FocusState / FocusedValue / Commands / keyboardShortcut verified APIs.
- `references/uikit-keyboard-focus.md` — UIKeyCommand + responder chain + UIFocusSystem verified APIs.
- `references/appkit-responder-commands.md` — NSResponder first-responder + NSMenu / NSMenuItem.keyEquivalent verified APIs.
- `references/web-keyboard-focus.md` — semantic elements + tabindex + :focus-visible + dialog + WAI-ARIA.
- `references/keyboard-accessibility.md` — keyboard-only operability; visible focus; no traps; WCAG 2.2 SC 2.1.1 / 2.4.7 / 2.1.2; ARIA roles; discoverability.

## Process notes

- The Skill does NOT introduce a fabricated Apple API symbol. Every symbol is verified in DocC + installed SDK per the inventory.
- The Skill does NOT make verbatim Apple HIG claims (HIG portal pages return JS-rendered shells in this session).
- The Skill does NOT assume cross-framework or cross-platform focus API parity.
- The Skill is verified by 20 functional tests (AE6-T1..T20) + 18 adversarial tests (AE6-G1..G18) + 12 router tests (AE6-R1..R12).
- The Skill consumes but does not redefine the v0.5 Skills (`apple-control-states`, `apple-motion-physics`, `apple-modality-overlays`, `apple-adaptive-structure`, `apple-feedback-response`, `apple-navigation-spatial`, `apple-direct-manipulation`).