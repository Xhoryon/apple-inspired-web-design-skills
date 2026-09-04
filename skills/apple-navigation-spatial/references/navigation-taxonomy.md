# Navigation Taxonomy Reference

The canonical navigation event vocabulary. This file is the Tier 2 reference for `apple-navigation-spatial`.

The Skill owns the semantic distinction between these events. The implementation uses the platform's standard primitives; the taxonomy tells you which event you are implementing.

## The six canonical navigation events

| Event | Semantic | Spatial relationship | Typical platform treatment | Inappropriate substitutions |
|---|---|---|---|---|
| Navigate (deeper) | The user has moved forward into a child of the current destination | Source is parent, destination is child (forward in the hierarchy) | Native: `UINavigationController.push`, `NavigationStack`. Web: URL change + animated transition. | Treating a deeper destination as a modal. Treating a modal as a deeper destination. |
| Navigate (peer) | The user has switched to a sibling destination | Source and destination are siblings; neither is parent of the other | Native: `UITabBarController`, `TabView`. Web: tab / sidebar element. | Pushing the previous tab onto a back stack. Treating peer as hierarchical. |
| Back | The user retraces the navigation history | Destination is the previous entry in the stack | Native: system Back gesture, ⌘[. Web: browser back. | Closing arbitrary UI. Dismissing a sheet. Calling Back a "Close" button. |
| Modal / present | A focused, distinct task temporarily supersedes the current context | Source remains in place; the modal overlays it | Native: `UISheetPresentationController`, `.sheet`. Web: dialog / sheet with focus trap. | Putting a modal in the URL history as a back destination. Treating dismissal as navigation. |
| Reveal / expand | Additional content appears within the current destination | The destination expands; no new destination is created | Native: `UIContextMenuInteraction`, expand-in-place. Web: inline disclosure. | Pushing the reveal onto the back stack. Treating the expansion as a new destination. |
| External | The user leaves the app / site | Source and destination are in different apps / sites | Native: `openURL`, share extension. Web: outbound link. | Calling external navigation a Back destination. |

## "Back" is not interchangeable with "Close" or "Dismiss"

These three verbs are different operations. They are not the same button.

- **Back** is hierarchical traversal. It moves the user to a previous entry in the navigation history. It does NOT close arbitrary UI.
- **Close** is dismissal. It closes a document, a window, or a temporary surface that is not a hierarchical destination.
- **Dismiss** is a separate verb. It cancels or removes a non-modal context (popover, menu, tooltip) without affecting navigation history.

A Web app that calls its browser-back replacement "Close" is wrong. A sheet that uses a "Back" button to dismiss is wrong. An iOS app that closes a tab on Back is wrong. The verbs have different effects on the user's context.

> **HARD RULE:** A single back button must not do double duty. If a surface is presented modally, it dismisses on its own affordance; the system Back on iOS may or may not dismiss it depending on whether the surface participates in the navigation stack. Browser back on Web is hierarchical traversal, not "close everything."

## When to use which

- **The user clicked a list item to see its detail.** Navigate (deeper). Browser back returns to the list with scroll position. Source: list view. Destination: detail view.
- **The user tapped a tab in a tab bar.** Navigate (peer). The previous tab is not pushed. There is no back-stack relationship between tabs.
- **The user opened a popover to change a setting.** Reveal. The popover is contextual, not a new destination. There is no back stack.
- **The user opened a sheet to edit a value.** Modal. The sheet is a focused task. Dismissing the sheet does not navigate back.
- **The user clicked an outbound link.** External. The browser leaves the current site. There is no in-app Back.
- **The user wants to leave the current view.** Back. The browser back / system Back gesture / ⌘[ returns to the previous entry in the stack.

## Source: where each event is initiated

A navigation event is initiated by a source element. The source element is the affordance that the user acted on (e.g. a list row, a tab, a button). The source element is also the focus-return target when the event is dismissed (see `focus-and-dismissal.md` in `apple-modality-overlays`).

A navigation event that has no source element (e.g. a programmatic `history.back()` call) is not a user-initiated event. The Skill does not require focus return for programmatic events; it requires it for user-initiated events.

## Stack vs replacement

Hierarchical push adds to the stack. Peer does NOT. Reveal does NOT. Modal does NOT (the modal sits on top of the stack, but the dismissal does not pop the stack).

| Event | Stack behavior | Back behavior |
|---|---|---|
| Navigate (deeper) | Pushes onto the stack | Pops the destination; returns to the source |
| Navigate (peer) | Replaces (or keeps in memory, platform-dependent) | Back does not return to the previous peer; it returns to the previous hierarchical entry |
| Back | Pops the stack | Triggers a new destination (the previous entry) |
| Modal / present | Does NOT push onto the hierarchical stack | The system Back may dismiss a non-modal-presented overlay; otherwise back traverses the underlying stack |
| Reveal / expand | Does NOT push | There is no back destination |
| External | Pushes to the system / browser history | Back returns to the previous external or in-app entry |

## Edge cases

- **Deep links** open a destination directly. The stack is reconstructed or replaced. Back from a deep-linked destination should return to a sensible default (the app's root, or the previous session's last entry).
- **Modal stacked on a deep-linked destination** — the modal is presented over the destination. The system Back may dismiss the modal first, then the destination. On Web, browser back must be made aware of modal state, or the URL must reflect both the destination and the modal state.
- **Reveal that opens a modal** — the reveal closes; the modal opens. There is no combined "reveal + modal" verb. Treat the transition as two separate events.

## Cross-Skill note

- The taxonomy is the contract. The implementation uses the platform primitive.
- Spatial continuity (visual relationship between source and destination) is in `spatial-continuity.md`.
- Scroll context chrome (navbar / title response) is in `scroll-behavior.md`.
- Modal type decision is in `apple-modality-overlays` (not here).
- Gesture-based dismissal of a modal is a cross-Skill concern: `apple-direct-manipulation` (gesture) + `apple-modality-overlays` (semantics) + `apple-motion-physics` (physics).
