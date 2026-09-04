# Keyboard Navigation

The Skill's rules for keyboard traversal across Apple platforms and Web. Traversal primitives vary per platform; the Skill records what each platform supports.

## Hard invariant

> **Do not implement an arbitrary universal Tab algorithm across all Apple platforms.**

The Skill records per-platform traversal primitives; it does NOT invent a single algorithm.

## Per-platform traversal primitives

| Platform | Forward | Backward | Group traversal | Cross-group | Notes |
|---|---|---|---|---|---|
| iOS / iPadOS (with hardware keyboard) | Tab (where focus API is supported) | Shift-Tab | arrow keys (lists / grids) | hardware-keyboard navigation + accessibility | Apps that opt into the focus API gain keyboard traversal |
| macOS | Tab | Shift-Tab | arrow keys | Full Keyboard Access | Tab traverses focusable elements; Full Keyboard Access adds cross-group traversal |
| tvOS | focus engine (remote-driven) | focus engine | focus engine | focus engine | Entire interaction model is focus-driven |
| visionOS | spatial focus (gaze + pinch) | spatial focus | spatial focus | spatial focus | Spatial primitives |
| watchOS | Digital Crown | Digital Crown | limited | n/a | Small screen |
| Web | Tab | Shift-Tab | arrow keys (lists / menus) | native focus order + skip links | HTML Living Standard + WAI-ARIA |

## Focus order rules

The Skill publishes the rule:

- Focus order should generally follow **visual / task logic**, then **semantic structure**, then **platform convention**.
- Do NOT create surprising focus jumps.
- Web: avoid positive `tabindex` choreography as a default. Prefer logical DOM / semantic order. W3C / WAI guidance applies.

## Group traversal

A focus group is a scope of focus (e.g. fields within a form section; items within a list). Within a group, Tab moves to the next focusable item. Across groups:

- macOS: Full Keyboard Access provides cross-group traversal; default Tab may or may not cross groups depending on the app.
- Web: native Tab follows DOM order; arrow keys are used in lists / menus.
- iOS / iPadOS: arrow keys in lists / grids; hardware-keyboard Tab traverses focusable elements when the app opts into the focus API.

## Text fields vs controls

- Text fields are natively focusable on all platforms.
- Tab in a form moves from one field to the next.
- Custom widgets (date pickers; rich text editors; sliders) are focusable via the platform's primitive.
- The Skill does NOT require custom Tab handling when the platform primitive provides it.

## Skip traps

The Skill publishes the rule: do NOT unintentionally trap keyboard focus.

- A true modal may constrain focus to its temporary interaction context (`<dialog showModal>` on Web; SwiftUI / UIKit / AppKit modal context).
- Closing / dismissing the modal must restore appropriate focus.
- Nonmodal regions must NOT act like accidental focus prisons.

## Modal containment

When a modal opens, focus moves into the modal. When the modal closes, focus returns to the invoking control or a sensible continuation.

See `references/focus-continuity.md` for restoration rules.

## Custom widgets

For custom focusable widgets:

- Use the platform's focus API (SwiftUI `.focusable`; UIKit `UIFocusItem`; Web `tabindex="0"` + `role`).
- Provide a visible focus indicator (system focus effect preferred; custom only when necessary).
- Ensure keyboard activation (Return / Space / Enter triggers the action).
- Do NOT invent a focus API; use the platform primitive.

## What the Skill rejects

- **Keyboard Trap** — user enters a widget / context by keyboard but cannot leave.
- **Positive `tabindex` choreography on Web** — using `tabindex="1"`, `tabindex="2"` to force focus order via DOM order override.
- **Surprising focus jumps** — focus jumping across unrelated sections.
- **Custom focus visuals that hide the indicator** — rejecting system focus effect for visual cleanliness.

## Reference

- `references/web-keyboard-focus.md` — Web-specific focus primitives.
- `references/keyboard-accessibility.md` — accessibility constraints on keyboard navigation.