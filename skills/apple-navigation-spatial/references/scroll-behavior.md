# Scroll Behavior Reference

Scroll is owned by the platform / browser. This file is the Tier 2 reference for `apple-navigation-spatial`.

> **HARD RULE:** Do not hijack native scrolling. The Skill defines scroll context chrome (navbar response, title collapse) and scroll-related Web behavior; it does NOT define scroll physics.

## The platform owns scroll

- **Native iOS / iPadOS / macOS:** UIKit / SwiftUI / AppKit scroll views. The system handles inertia, deceleration, edge resistance, and gesture recognition. A Skill that overrides these is wrong.
- **Web:** the browser handles wheel, touch, and keyboard scroll. The Skill does NOT call `preventDefault` on `wheel`, `scroll`, or `touchmove` to override native scrolling. CSS `scroll-behavior: smooth` and `scroll-snap` are platform-native; they are not scroll hijacking.

A Skill that prevents default scrolling is a defect. The user expects native scroll; the platform provides it; the Skill works with it.

## What the Skill owns

The Skill owns:

- **Navigation chrome response to scroll** — what the navbar, title, and contextual toolbar do in response to scroll position.
- **Title collapse** — large title → small title as the user scrolls.
- **Toolbar behavior** — contextual toolbar that appears at scroll thresholds.
- **Scroll restoration** — preserving scroll position across navigation events.
- **Sticky controls** — persistent UI elements that remain visible during scroll.
- **Reduced-motion adaptation** — chrome transitions respect `prefers-reduced-motion: reduce`.
- **Performance** — chrome updates are efficient; layout is not thrashed on every scroll event.

The Skill does NOT own:

- Scroll physics (inertia, deceleration, bounce).
- Custom rubber-band / edge resistance.
- Pull-to-refresh behavior (this is a system gesture, see `apple-direct-manipulation/references/gesture-conflicts.md`).
- Edge-swipe back (this is a system gesture, ABANDON on Web).

## Navigation chrome response

A navigation chrome may respond to scroll in one of several ways:

- **Collapse on scroll down, expand on scroll up.** The header shrinks as the user scrolls down; it grows back as the user scrolls up. This is the canonical iOS pattern.
- **Hide on scroll down, show on scroll up.** The header is hidden when the user scrolls down; it is shown when the user scrolls up. Less common; used when the content needs more vertical space.
- **Always visible.** The header is sticky and does not respond to scroll. This is acceptable for short pages or for the navigation chrome of a sub-page.
- **Contextual appearance.** A contextual toolbar (e.g. "Add", "Edit") appears when the user has scrolled past a specific content section. The toolbar disappears when the user scrolls back.

### Reversibility

Scroll-linked chrome must be reversible. A user who scrolls down and then scrolls up must see the chrome return to its previous state. A chrome that is hidden by scrolling and never reappears is a defect.

### State-driven, not gesture-driven

The chrome response is state-driven (driven by `scrollY` / scroll position), not gesture-driven (driven by the end of a flick). A scroll position of 100 px produces the same chrome state regardless of how the user got there.

### Accessibility

The chrome response is accessibility-friendly:

- The user can scroll normally; the chrome does not prevent scroll.
- The chrome state does not affect the focus order.
- The chrome respects `prefers-reduced-motion: reduce` — the transition between expanded and collapsed states is shorter or absent.

## Title collapse

Large title → small title is a specific case of navigation chrome response. As the user scrolls, the large title (top of the page) shrinks to a small inline title (in the navigation bar). This is the canonical iOS pattern.

The collapse threshold is an implementation heuristic. Common values are 30–60 px of scroll, depending on the height of the large title. The Skill does not publish a canonical value.

## Contextual toolbars

A contextual toolbar appears at a specific scroll position. Example: a "Buy" button appears when the user has scrolled past the product description to the reviews section.

The toolbar is:

- Triggered by a scroll position threshold.
- Smoothly animated in / out.
- Reversible (scroll back to dismiss).
- Respectful of `prefers-reduced-motion: reduce`.
- Not a replacement for always-visible actions; always-visible actions stay visible.

## Sticky controls

Sticky controls are UI elements that remain visible during scroll. Examples:

- A "Buy" button at the bottom of a product page.
- A search bar at the top of a list.
- A toolbar at the bottom of a content view.

A sticky control:

- Does not prevent scroll.
- Is accessibility-friendly (the user can scroll the content behind it).
- Has a `background` (or `backdrop-filter`) that distinguishes it from the content behind it.
- Does not overlap the content's text or interactive elements (edge legibility).

A sticky control that is transparent and has no background is a defect — the user cannot distinguish the control from the content.

## Scroll restoration

Scroll restoration preserves the scroll position across navigation events. The user navigates from list A to detail A1; the user goes back; the list is at the same scroll position.

- **Native iOS / iPadOS:** the system restores scroll position when the user returns to a view controller. UIKit / SwiftUI handle this.
- **Web:** the browser restores scroll position on browser back / forward in most modern browsers. The Skill does not need to implement this; the platform does it.
- **Web single-page apps (SPA):** the SPA must implement scroll restoration itself, because the browser does not know the SPA's navigation model. The SPA stores the scroll position when leaving a view; it restores the position when returning.

Scroll restoration is the platform's responsibility, except for SPAs. The Skill does not write custom scroll restoration code for non-SPA Web.

## Edge legibility

When a sticky control is present, the content under the control must remain readable. Common patterns:

- The control has an opaque or solid background.
- The control has a `backdrop-filter` blur (Liquid Glass on Web — see the stable v1.1 `apple-liquid-glass-web`).
- The content has padding at the top / bottom equal to the control's height.

A sticky control over a long paragraph with no padding is a defect. The user cannot read the paragraph's first / last line.

## Performance

Scroll events fire at high frequency (60–120 Hz). Chrome that updates on every scroll event can thrash layout. The Skill recommends:

- Use `requestAnimationFrame` to batch updates.
- Use `transform` and `opacity` for animation; avoid `width`, `height`, `top` (these cause layout).
- Use `passive: true` on scroll / touch listeners to allow the browser to optimize.
- Use the `IntersectionObserver` API for scroll-position thresholds instead of computing on every scroll event.
- For long lists, use virtualization (the stable v1.1 `apple-ui-components` may have a reference).

## Reduced motion

Chrome transitions must respect `prefers-reduced-motion: reduce`:

- The transition between expanded and collapsed states is shorter (or absent).
- The transition is still perceivable; the state change is visible.
- `auto` keyword on `scroll-behavior` (e.g. `scroll-behavior: smooth auto`) lets the browser decide based on user preference.

## What the Skill rejects (ABANDON)

- **Fake rubber-band / edge resistance on Web.** A Web app that prevents default scroll and adds custom bounce is wrong. Use the platform's scroll; if the user wants iOS-style bounce, use iOS.
- **Custom pull-to-refresh on Web.** The system gesture is platform-native. Custom Web implementations are a system-gesture hijack (see `apple-direct-manipulation/references/gesture-conflicts.md`).
- **Custom edge-swipe back on Web.** The browser back is canonical. A custom edge swipe conflicts with browser back and horizontal scroll.
- **Custom scroll physics.** The platform handles it. Web scroll is the browser's; native scroll is the OS's.

## Cross-Skill note

- The navigation taxonomy is `navigation-taxonomy.md`.
- The spatial continuity is `spatial-continuity.md`.
- The pull-to-refresh and edge-swipe conflicts are `apple-direct-manipulation/references/gesture-conflicts.md`.
- The motion physics of chrome transitions is `apple-motion-physics`.
- The `prefers-reduced-motion` rule is `apple-motion-physics/references/reduced-motion.md`.
