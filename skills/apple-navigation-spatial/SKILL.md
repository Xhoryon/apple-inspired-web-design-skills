---
name: apple-navigation-spatial
description: Use when designing or implementing navigation hierarchy, spatial continuity, back semantics, scroll context chrome, and the distinction between navigate / select / reveal / present / expand. Applies to Native iOS / iPadOS / macOS (system primitives preferred) and Desktop Web (browser history + URL + animated transitions). Required for list-to-detail navigation, tab/peer navigation, modal presentation, scroll restoration, and spatial continuity between source and destination. Do NOT use for control state semantics (use apple-control-states), pointer-target relationship (use apple-pointer-interaction), motion physics (use apple-motion-physics), gesture recognition (use apple-direct-manipulation), or modal type decision (use apple-modality-overlays). Do NOT hijack native browser scrolling or implement custom edge-swipe back on Web.
version: "0.1.0-dev"
license: MIT
---

# apple-navigation-spatial

Navigation hierarchy + spatial continuity. The Skill owns WHERE the user is going and HOW the interface communicates movement between places.

## When to use

Use when the task involves navigation hierarchy, modal presentation, or spatial continuity. Symptoms:

- "Click a list item, go to detail, back should return to the list with scroll position."
- "Switch between tabs — the previous tab should not 'push' onto a stack."
- "When the user goes from list to detail, the detail should 'come from' the list item."
- "When the user dismisses a modal, focus should return to the trigger."
- "The navbar should condense on scroll down and reappear on scroll up."
- "Implement iOS edge swipe back on Web."

Do **not** use when:

- The task is control state semantics (idle / hover / pressed / focused / selected / disabled). Use `apple-control-states`.
- The task is pointer-target relationship (hover highlight, shared highlight). Use `apple-pointer-interaction`.
- The task is motion physics (spring, interruptibility). Use `apple-motion-physics`.
- The task is gesture recognition (tap, drag, swipe, pinch). Use `apple-direct-manipulation`.
- The task is choosing between sheet / popover / menu / alert / action sheet. Use `apple-modality-overlays`.

## Hard invariants

1. **Back is hierarchical traversal.** Back retraces the navigation history. It does not close arbitrary UI, clear selection, or dismiss unrelated UI. Close / Dismiss is a separate verb.
2. **System primitive first.** On Native iOS / iPadOS / macOS, navigation is provided by UINavigationController / NavigationStack / SwiftUI navigation, by UITabBarController / TabView, and by the system Back gesture. Custom navigation chrome is rarely justified.
3. **Scroll is owned by the platform.** On Native Apple, scroll is owned by UIKit / SwiftUI. On Web, scroll is owned by the browser. The Skill defines scroll context chrome (navbar response, title collapse), NOT scroll physics.
4. **Spatial continuity preserves context.** The user must know where they came from and where they went. The transition or layout reinforces this (shared element, source anchoring, back button).
5. **No scroll hijacking.** On Web, do NOT preventDefault on wheel / scroll / touchmove for the purpose of overriding native scrolling. Scroll-linked UI is allowed (navbar response, contextual toolbar), but native scrolling remains.
6. **No custom edge-swipe back on Web.** Custom edge-swipe back conflicts with browser navigation, horizontal scroll, and accessibility gestures. Use the browser's native Back.
7. **Modal presentation is not hierarchical.** A sheet / modal is not "deeper" in the hierarchy; it is a temporary, focused task overlay. Pressing "Done" or dismissing does not navigate back in the hierarchy.

## Navigation model

A canonical navigation event is one of:

- **Navigate (deeper)** — push a new destination onto the stack. The user has moved forward in a hierarchy. Back goes to the previous destination.
- **Navigate (peer)** — switch to a sibling destination (tab / sidebar item). The previous destination is NOT pushed onto a stack; it is replaced (or kept in memory by the platform, depending on architecture).
- **Back** — retrace the navigation history. Browser-native on Web, system-native on iOS / iPadOS.
- **Modal** — present a focused task overlay. The hierarchy behind the modal is preserved; dismissing the modal does not navigate back.
- **Reveal / expand** — show additional content within the current destination. Not a new destination; no Back.
- **External** — leave the app / site. Browser-native.

Each navigation event has a different transition. The Skill records the transition rules; the implementation uses the platform's standard primitives.

## Spatial continuity

When transitioning from A to B, the user should understand the relationship. Spatial continuity rules:

- The destination's visual position suggests the source's location (e.g. card → detail card from the same position).
- The selected state in A persists into B if relevant (selected tab → its content; selected list item → detail).
- The scroll position in A may restore when the user returns from B (browser-native on Web; system-native on iPadOS multi-window).
- The toolbar / navigation chrome should be consistent across A and B (same navigation surface, different content).

Spatial continuity is NOT about copying a shared element. It is about preserving context.

## Back semantics

Back is hierarchical traversal. It does NOT:

- Close arbitrary UI (use Close / Dismiss).
- Clear selection (use a Clear action).
- Dismiss unrelated popovers (use a dismiss action).

If the user has multiple levels of navigation history, Back traverses them in reverse order. The browser's Back is canonical on Web. The system Back gesture is canonical on iOS / iPadOS.

Close / Dismiss is a separate verb with separate semantics. The Skill must NOT conflate "Back" with "Close" or "Dismiss."

## Scroll context chrome

Scroll context chrome is the navigation UI that responds to the user's scroll position. Examples:

- A navbar that condenses on scroll down and re-expands on scroll up.
- A title that shrinks as the user scrolls past it.
- A contextual toolbar that appears when the user scrolls a particular distance.
- A scroll progress indicator.

Scroll context chrome is reversible, state-driven, and accessibility-friendly. It does NOT prevent the user from scrolling. It is NOT scroll hijacking.

Scroll context chrome does NOT define scroll physics. Scroll is owned by the platform.

## Native Apple standard controls

On Native iOS / iPadOS / macOS, the Skill recommends:

- UINavigationController / SwiftUI NavigationStack for hierarchical navigation.
- UITabBarController / SwiftUI TabView for peer navigation.
- The system Back gesture (iOS / iPadOS edge swipe, macOS Esc, ⌘[) for back.
- The system scroll physics (UIKit / SwiftUI scroll views).
- UISheetPresentationController / SwiftUI .sheet for modal sheets.
- UIPopoverPresentationController for popovers.
- UIContextMenuInteraction for context menus.
- UIAlertController for alerts and action sheets.

Custom navigation chrome is rarely justified on Apple platforms. The Skill recommends the system primitive.

## Web navigation

On Web, the Skill enforces:

- Browser back works (browser history is canonical).
- URL is meaningful and deep-linkable.
- Focus moves predictably on navigation.
- Scroll restoration is handled by the platform (the browser restores scroll position on back / forward by default in most modern browsers).
- Modal overlays do not break URL semantics; the URL should reflect the route, not the modal.
- Deep links work: a URL can open the app at a specific destination.

Apple-inspired motion (shared element, source anchoring) must NOT break Web navigation semantics. The animation is decoration; the navigation is the URL.

## Scroll behavior reference (Tier 2)

The reference `references/scroll-behavior.md` covers:

- Content scroll: native platform / browser scroll.
- Navigation chrome response: navbar / title / contextual toolbar in response to scroll.
- Title collapse: large title → small title as the user scrolls.
- Toolbar behavior: contextual toolbar that appears at scroll thresholds.
- Edge legibility: ensuring the content is not under a sticky chrome.
- Scroll restoration: native on Web back / forward; native on iPadOS multi-window.
- Sticky controls: persistent UI elements; NOT scroll jacking.
- Content underlay: content below sticky chrome.
- Momentum ownership: native platform / browser owns momentum.
- Reduced motion: chrome transitions respect `prefers-reduced-motion: reduce`.

## Ownership boundaries

- **Navigation semantics** (push, peer, back, modal, reveal, expand) — `apple-navigation-spatial`.
- **Spatial relationship** (where things are on the screen, source / destination relationship) — `apple-navigation-spatial`.
- **Scroll response** (chrome response to scroll) — `apple-navigation-spatial` (reference).
- **Scroll physics** (native vs. custom) — platform, NOT a Skill. Native iOS scroll, native browser scroll. The Skill does NOT recommend custom scroll physics.
- **Modal type decision** (sheet vs. popover vs. menu) — `apple-modality-overlays`.
- **Modal dismissal** (focus return, unsaved changes) — `apple-modality-overlays`.
- **Press / hover / selected state** — `apple-control-states`.
- **Pointer-target relationship** — `apple-pointer-interaction`.
- **Motion physics** (spring, interruptibility) — `apple-motion-physics`.

## Reference catalog (Tier 2)

- `references/navigation-taxonomy.md` — navigate, peer, back, modal, reveal, expand.
- `references/spatial-continuity.md` — source / destination relationship, scroll restoration.
- `references/scroll-behavior.md` — scroll ownership, scroll context chrome, scroll restoration.

## Hard invariants (recap)

1. Back is hierarchical traversal (HARD).
2. System primitive first (HARD).
3. Scroll is owned by the platform (HARD).
4. Spatial continuity preserves context (HARD).
5. No scroll hijacking (HARD).
6. No custom edge-swipe back on Web (HARD).
7. Modal is not hierarchical (HARD).
8. The 7 stable v1.1 Skills are READ-ONLY.

## Companion files

- `references/navigation-taxonomy.md`
- `references/spatial-continuity.md`
- `references/scroll-behavior.md`
