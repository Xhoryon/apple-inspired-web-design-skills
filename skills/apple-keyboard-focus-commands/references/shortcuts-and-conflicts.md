# Shortcuts & Conflicts

The Skill's rules for keyboard shortcuts. **A shortcut is one invocation path; an important command should also be discoverable through menu / toolbar / context menu / button.**

## Hard principles

- **Do NOT casually repurpose familiar system-standard shortcuts for unrelated actions.** Standard shortcuts have platform-conventional meanings.
- **Do NOT invent shortcut priority algorithms.** Priority follows platform convention + scope + frequency + discoverability.
- **Do NOT require memorization as the only discoverability path for important functionality.** Important commands should be accessible through menu / toolbar / context menu / button.

## Standard shortcuts (preserve)

Apple documents a set of standard shortcuts. The Skill preserves their conventional meanings:

| Shortcut | Conventional meaning |
|---|---|
| ⌘C | Copy |
| ⌘V | Paste |
| ⌘X | Cut |
| ⌘A | Select All |
| ⌘Z | Undo |
| ⇧⌘Z | Redo |
| ⌘S | Save |
| ⌘Q | Quit |
| ⌘W | Close Window |
| ⌘N | New |
| ⌘O | Open |
| ⌘P | Print |
| ⌘F | Find |
| ⌘, | Preferences |
| ⌘Q | Quit |

Standard shortcuts on Web follow browser / OS conventions:

| Shortcut | Conventional meaning |
|---|---|
| Ctrl/Cmd + C | Copy |
| Ctrl/Cmd + V | Paste |
| Ctrl/Cmd + X | Cut |
| Ctrl/Cmd + A | Select All |
| Ctrl/Cmd + Z | Undo |
| Ctrl + Shift + Z / Ctrl + Y | Redo |
| Ctrl/Cmd + S | Save (web app convention) |
| Ctrl/Cmd + F | Find |
| F1 | Help |
| Tab / Shift-Tab | Focus next / previous |
| Enter / Return | Default action |
| Escape | Cancel / dismiss |

The Skill does NOT reimplement the full standard shortcut table in Tier 1 — that goes in Tier 2 reference.

## Custom shortcuts

A custom shortcut is appropriate when:

- The action is **frequent**.
- **Standard shortcuts don't conflict**.
- The shortcut is **discoverable** through menu / tooltip / command surface.
- The shortcut **respects keyboard layout / locale**.

The Skill does NOT prescribe "secondary commands use Option-Command" or any other shortcut formula. Shortcut choice is contextual.

## Shortcut conflict resolution

When a shortcut conflict exists:

1. **System-reserved shortcuts** take priority. The app does NOT override them.
2. **Standard application commands** take priority over custom commands.
3. **Custom frequent commands** may override less-frequent ones if context demands.
4. **Text-input conflicts** — a shortcut that conflicts with a text input's standard behavior (e.g. ⌘C inside a text field) is handled by the text input, not the app.
5. **Keyboard-layout differences** — the app may need to handle Cmd (macOS) vs Ctrl (Windows / Linux) for cross-platform desktop Web.
6. **Modifier conflicts** — record which modifier combinations are in use.
7. **Local vs app-wide scope** — a shortcut may be local to a focused context vs global.

The Skill does NOT invent a "priority algorithm". The app designer makes context-appropriate choices.

## Shortcut discoverability

A shortcut should not require memorization as the only way to discover critical functionality. Discoverability channels:

- **Menu item** — macOS menu bar shows shortcut hints (⌘S); iOS shows shortcuts in the keyboard bar.
- **Tooltip / help** — hover / long-press reveals the shortcut.
- **Command interface** — command palette (if product-appropriate).
- **Keyboard shortcut display** — platform-provided UI (e.g. macOS shortcut hint in menu).
- **Discoverability surface** — first-run / settings panel.

The Skill does NOT invent a universal "hold Command overlay" behavior for all platforms.

## Localized shortcut display

The Skill publishes the rule:

- Do NOT assume physical key position equals character identity (e.g. Y vs Z on QWERTZ vs QWERTY).
- Current API behavior can account for keyboard layouts / localization.
- Canonical principles should avoid ASCII / ANSI-only assumptions.

## What the Skill rejects

- **Shortcut Hijacking** — repurposing well-known system commands for unrelated actions.
- **Shortcut-Only Essential Action** — important action exists only as hidden key combination.
- **Shortcut Numerology** — invented canonical numeric formula for shortcut choice.
- **Standard Shortcut Repurposing** — using ⌘S for "Share" because "Save isn't important."

## Reference

- `references/command-scope.md` — command scope + enablement consistency.
- `references/swiftui-focus-commands.md` — SwiftUI `keyboardShortcut(_:)` modifier.
- `references/appkit-responder-commands.md` — AppKit `NSMenuItem.keyEquivalent` + menu validation.
- `references/web-keyboard-focus.md` — Web shortcut + system-reserved keys.
- `references/keyboard-accessibility.md` — accessibility + discoverability.