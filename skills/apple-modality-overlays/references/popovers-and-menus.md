# Popovers and Menus Reference

Popover and menu semantics, anchoring, narrow-viewport adaptation. This file is the Tier 2 reference for `apple-modality-overlays`.

> **HARD RULE:** Popover is for contextual supplementary UI. Menu is for commands / actions. They are NOT multi-step forms.

## Popover

A popover is an anchored contextual supplementary UI. It is:

- Lightweight (not a multi-step form).
- Anchored to a source element (the popover appears near the source).
- Dismissable (tap outside, Escape on Web, system Back on iOS).
- Returns focus to the source on dismiss.

### When a popover is appropriate

- A contextual action surface tied to a specific element (e.g. a settings popover anchored to a gear icon).
- A supplementary information surface (e.g. a date picker popover anchored to a date field).
- A quick-choice surface (e.g. a list of options anchored to a button).
- A preview surface (e.g. an image preview anchored to a thumbnail).

### When a popover is NOT appropriate

- Multi-step forms. (Use a sheet.)
- Long content. (Use a sheet or a navigation destination.)
- Critical information. (Use an alert.)
- Success messages. (Use inline status.)
- Commands that have nothing to do with the source. (Use a menu / context menu.)

### Popover geometry

The popover:

- Emerges from the source element's bounding box.
- Has a transform origin at the source.
- Has a maximum size (e.g. 320 × 480 px on iOS; the implementer chooses).
- Adapts to the available space (see narrow-viewport adaptation below).

The exact geometry is HEURISTIC. The principle is hard: the popover is anchored to the source.

### Popover dismissal

The popover is dismissed by:

- **Tap on a popover item** that performs an action.
- **Tap outside the popover.**
- **Escape on Web.**
- **System Back on iOS (depending on whether the popover is in the navigation stack).**

The popover returns focus to the source on dismiss. See `focus-and-dismissal.md`.

### Narrow-viewport adaptation

On a narrow viewport (touch Web, mobile browser, iPhone), the popover becomes a sheet. The reasoning:

- A popover needs horizontal space to appear next to the source. On a narrow viewport, the available space is too small.
- A popover needs a fine pointer to interact with its items. On a touch-only device, the user has no fine pointer; the popover would be a small tap target.
- A sheet is the touch-appropriate presentation for the same content.

The adaptation is automatic (the Web framework may handle it; otherwise, the implementer detects the viewport and changes the presentation).

The Skill records this as ADAPT (Desktop Web → touch Web). The popover becomes a sheet on narrow viewports.

## Menu

A menu is a list of commands / actions. It is:

- A command surface, NOT a form.
- Triggered by a button, a keyboard shortcut, a right-click, or a long-press.
- Dismissable.
- Returns focus to the trigger on dismiss.

### When a menu is appropriate

- A list of commands tied to the current context (e.g. "Edit > Copy").
- A list of actions on a selected element (e.g. right-click on a file: Open, Rename, Move, Delete).
- A list of choices for a setting (e.g. View > Zoom > 100%, 125%, 150%).

### When a menu is NOT appropriate

- A multi-step form. (Use a sheet.)
- A long list of options that should be searchable. (Use a picker or a search field.)
- A primary navigation surface. (Use a navigation menu / tab bar.)
- A contextual action tied to a specific element. (Use a popover or context menu — the menu is triggered by the element, but the menu is not anchored to it.)

### Menu / context menu

A **menu** is a top-level or top-bar menu (e.g. "File > Open"). A **context menu** is a menu that appears on right-click (mouse), long-press (touch), or ctrl-click (macOS).

The context menu is:

- Tied to the element that received the gesture.
- A list of commands applicable to the element.
- Dismissable.

The context menu is `UIContextMenuInteraction` on iOS / iPadOS. On Web, the context menu is a `<menu>` element or a custom DOM tree shown on `contextmenu` event.

## Desktop pointer adaptation

On Desktop Web (mouse / trackpad):

- Right-click on an element shows a context menu.
- The context menu is anchored to the pointer position.
- The user can dismiss with Escape or by clicking outside.

On Desktop macOS:

- Ctrl-click on a trackpad shows a context menu.
- Force-click shows a context menu (on supported hardware).

## Keyboard adaptation

On Desktop Web and on macOS:

- The user can navigate a menu with arrow keys.
- Enter / Space activates the focused item.
- Escape closes the menu.
- A keyboard shortcut (e.g. Cmd+C for Copy) is a fast path.

The Skill recommends keyboard support for all menus. The user expects it.

## Touch adaptation

On Touch Web:

- Long-press on an element shows a context menu.
- The context menu is positioned at the touch location (or adapted to the available space).
- Tap on a menu item activates the action.
- Tap outside the menu dismisses it.

The Web platform's native context menu (browser's default right-click) may be replaced by a custom menu. The Skill recommends a custom menu that matches the brand and the design system.

## Narrow viewport adaptation

On a narrow viewport:

- A popover becomes a sheet (see above).
- A context menu becomes a bottom sheet or a full-screen menu.
- A top-bar menu becomes a navigation drawer or a hamburger menu.

The adaptation is automatic (the framework handles it; otherwise, the implementer detects the viewport and changes the presentation).

## Menus are not multi-step forms

A menu is a list of commands. A command is a single action. The user activates a command; the menu dismisses; the action runs.

A menu that asks the user to fill in a form is wrong. A menu that requires the user to make multiple selections is wrong. A menu that opens another menu (a "submenu") is acceptable but should be used sparingly.

The Skill rejects menus as multi-step forms. Use a sheet for that.

## Cross-Skill note

- The decision tree (when to use a popover / menu) is `modality-decision-tree.md`.
- The sheet semantics are `sheets.md`.
- The alert / confirmation distinction is `alerts-and-confirmation.md`.
- The focus return is `focus-and-dismissal.md`.
- The source continuity is `source-continuity.md`.
- The control state semantics (selected, focused) are `apple-control-states`.
