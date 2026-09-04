# Structural Taxonomy

The eight Skill-owned structural verbs. Each verb describes an INTENT for how anatomy reorganizes when the layout adapts. The verb set is an **Apple Experience Skill design vocabulary** — useful for reasoning about structure, but NOT extracted from Apple HIG as a literal primitive set. The Skill explicitly labels these as its own vocabulary so they cannot be misattributed to Apple as canonical primitives.

The verbs are also distinct from SwiftUI / UIKit API names — they describe intent, not symbols. Implementation uses the documented platform primitives (`NavigationSplitView`, `UISplitViewController.Style`, container queries, etc.).

## Verb set (Skill-owned)

| Verb | Meaning | Source of vocabulary | Web equivalent (intent) |
|---|---|---|---|
| **inherit** | Keep the same anatomy at the new context. The default verb. | Skill vocabulary (default) | "no layout change" |
| **slide** | Bring a child surface into view from an edge (e.g. a popover anchored to a button). | Skill vocabulary | CSS transform translate + transition |
| **push** | Push a new surface onto the navigation stack. Drill-in from list to detail. | Skill vocabulary | Browser-native history push |
| **replace** | Replace the current content at the same position. Peer / tab switching. Previous content preserved by the platform. | Skill vocabulary | In-page route replacement (no history entry) |
| **on-demand** | Show a surface only when the user requests it (e.g. an inspector via a toolbar button). | Skill vocabulary | Hidden by default; toggled via JS / CSS |
| **reflow** | The same anatomy reorganizes into a different form (e.g. 3-column → 2-column at narrower width; sidebar → tab bar). | Skill vocabulary | CSS Grid / Container Queries |
| **collapse** | Remove a surface from the layout; preserve its content in the navigation stack or off-screen state. | Skill vocabulary | Media query / container query hides the surface; content preserved in app state |
| **abandon** | The anatomy is incompatible with the new context. The structural element is not represented. The Skill's LAST resort verb. | Skill vocabulary | Element not in the DOM at this size |

## Verb selection rules

The verb selection is a 5-step decision:

1. **Is the new context larger?** If yes, the verb tends to `reflow` (more columns / more surfaces) or `on-demand` (additional inspector). If no, the verb tends to `collapse` or `reflow` (fewer columns).
2. **Is the input context different?** If the new context is `pointer` and the current is `touch`, the verb tends to `reflow` (expose hover affordances structurally). If the new is `focus-driven` (tvOS), the verb tends to `abandon` for surface chrome that is not focus-friendly.
3. **Is the window state different?** If the new window state is `multi-window` or `stage-manager`, the verb tends to `reflow` (resize the columns to the new width). If the new state is `popover-anchored`, the verb tends to `collapse` (the parent surface is the only one).
4. **What is the user's preference?** If the user has manually overridden the structural choice (e.g. "always sidebar"), the verb tends to `inherit` even at narrower widths. The Skill exposes the override.
5. **What does the platform support?** If the platform does NOT support the verb, the Skill returns ADAPT or ABANDON. Example: `collapse` is not natively supported on iPhone (iPhone has no sidebar to collapse); the Skill returns ADAPT (drill-in via navigation) on iPhone.

## Verb × platform matrix (canonical)

| Verb | iPhone | iPad | Mac Catalyst | Mac | visionOS | tvOS | watchOS | Web |
|---|---|---|---|---|---|---|---|---|
| inherit | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| slide | ✓ (popover) | ✓ (popover, sidebar slide) | ✓ | ✓ | — (no analog) | — (no analog) | — (no analog) | ✓ (transform) |
| push | ✓ (NavigationStack) | ✓ (NavigationStack inside detail) | ✓ | ✓ | ✓ (NavigationStack) | ✓ (focus-driven) | ✓ (NavigationStack) | ✓ (browser history) |
| replace | ✓ (TabView) | ✓ (TabView / NavigationSplitView) | ✓ | ✓ | ✓ (TabView ornament) | — (focus-driven only) | — (stack only) | ✓ (in-page route) |
| on-demand | ✓ | ✓ (inspector) | ✓ | ✓ | ✓ (`.ornament`) | — (always focus-driven) | — (not enough room) | ✓ (toggle) |
| reflow | ✓ (limited — 1 → 1) | ✓ (3 → 2 → 1) | ✓ (3 → 2 → 1) | ✓ (windowed → full-screen) | ✓ (WindowGroup resize) | — (focus-driven) | — (stack-only) | ✓ (Container Queries / Grid) |
| collapse | ✓ (drill-in via NavigationStack) | ✓ (sidebar → tab bar / back button) | ✓ (sidebar collapse) | ✓ (window resize) | ✓ (ornament dismiss) | — (no analog) | — (stack-only) | ✓ (media query) |
| abandon | ✓ (popover, inspector) | ✓ (popover, inspector at compact) | ✓ (popover) | ✓ (popover) | ✓ (iPad-style chrome) | ✓ (non-focus chrome) | ✓ (iPad-style chrome) | ✓ (DOM removed) |

## Verb × concern ownership

The Skill owns the STRUCTURAL verb. It does NOT own:

- The transition between verbs (use `apple-motion-physics`).
- The state of a control inside the structural surface (use `apple-control-states`).
- The pointer highlight or hover (use `apple-pointer-interaction`).
- The drag / pinch / swipe (use `apple-direct-manipulation`).
- The sheet / popover / menu semantics (use `apple-modality-overlays`).
- The material behavior (use `apple-liquid-glass-fidelity`).
- The navigation hierarchy (use `apple-navigation-spatial`).
- The page-level composition (use stable `apple-web-composition`).
- The component grammar (use stable `apple-ui-components`).

## When to use which verb (decision tree)

```
1. Is the new context larger?
   YES → reflow (more columns) or on-demand (inspector)
   NO  → continue

2. Is the input context different?
   pointer ↔ touch: reflow (expose hover affordances)
   focus-driven (tvOS): abandon (non-focus chrome)
   gaze-driven (visionOS): reflow (spatial)
   YES (other) → reflow
   NO  → continue

3. Is the window state different?
   multi-window / stage-manager: reflow (resize)
   popover-anchored: collapse
   YES (other) → reflow
   NO  → continue

4. Does the user have a manual preference?
   YES (e.g. "always sidebar") → inherit
   NO  → continue

5. Does the platform support the verb at the new size class?
   YES → use the verb
   NO  → ADAPT (find a platform-appropriate replacement) or ABANDON (if no replacement)
```

## Anti-patterns

- **Always inherit** — never reflowing. The user resizes the window and the layout does not adapt. Rejected.
- **Always reflow** — reflowing at every pixel. Performance + animation cost. Rejected; the Skill pairs reflow with stable width buckets.
- **Abandon by default** — collapsing structural surfaces that the platform supports. Rejected. The Skill reserves `abandon` for the case where the new context genuinely cannot represent the surface.
- **Modal-as-adaptive** — opening a sheet to substitute for a missing column. Rejected. Sheets are for focused tasks, not for missing anatomy.
- **Orientation-driven reflow** — reflowing on `orientationchange` instead of `resize`. Rejected. Width is the driver.

## Web implementation cheat sheet

| Verb | CSS primitive | Notes |
|---|---|---|
| inherit | (no CSS primitive) | No layout change. |
| slide | `transform: translate(...)` + `transition: transform` | Use the View Transitions API where supported. |
| push | `history.pushState()` + SPA router | Browser-native. |
| replace | `history.replaceState()` + SPA router | No history entry. |
| on-demand | `:popover-open` or `[aria-expanded="true"]` | The Popover API is the canonical Web primitive. |
| reflow | `@container (...)`, CSS Grid `grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))`, `display: none` | Container queries are per-component; viewport media queries are page-level. |
| collapse | `display: none` (or `visibility: hidden` to preserve layout) + off-screen state in JS | The collapsed content is preserved in app state. |
| abandon | `display: none` + content not in DOM | The content is not represented at this size. |

## Native Apple implementation cheat sheet

| Verb | SwiftUI primitive | UIKit primitive | AppKit primitive |
|---|---|---|---|
| inherit | (no change) | (no change) | (no change) |
| slide | `.transition(.move(edge:))` | UIViewControllerTransitionCoordinator | NSView animation |
| push | `NavigationStack` | `UINavigationController` | `NSWindowController` (sheet-style) |
| replace | `TabView` (selection) | `UITabBarController` | `NSWindow` replacement |
| on-demand | `inspector(isPresented:)` | custom inspector | `NSPopover` |
| reflow | `NavigationSplitView(columnVisibility:)` | `UISplitViewController(preferredDisplayMode:)` | `NSSplitViewController` |
| collapse | `NavigationStack` inside `NavigationSplitView` detail | `UINavigationController` from sidebar | sheet-style window |
| abandon | (not rendered) | (not rendered) | (not rendered) |

## What the Skill rejects

- A custom structural verb (e.g. "rotate") added to the canonical set without a written justification. The Skill preserves the eight canonical verbs.
- A verb that does not preserve task continuity. Example: a verb that destroys the back stack on `reflow` is rejected.
- A verb that violates structural honesty. Example: a verb that turns a tab bar into a content card is rejected.
- A verb that is implemented on top of an existing system primitive without a justification. The Skill prefers the system primitive.
