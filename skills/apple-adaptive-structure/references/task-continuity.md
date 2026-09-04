# Task Continuity

The Skill's task-continuity invariant: structural change must not silently destroy the open task. This reference documents the four pillars of task continuity and the platform-specific implementation.

## The four pillars

1. **Focus preservation** — the focused element remains focused across structural change.
2. **Selection preservation** — the selected element(s) remain selected across structural change.
3. **Scroll position preservation** — the scroll position of the relevant surface is preserved (or restored) across structural change.
4. **Back stack integrity** — the navigation back stack is intact across structural change; collapsing a surface does not clear the drill-in.

## Why task continuity matters

The principle "structural change should preserve the user's task context" is a **Skill-internal design principle** synthesized from general adaptive-design guidance. The user should not have to re-find their context after a structural change. The Skill does NOT quote a literal Apple HIG sentence here; the principle is derived from the observable behavior that system-managed structural transitions (e.g. `NavigationSplitView` collapse/expand) preserve focus, selection, and scroll position by default, and that any custom structural change should follow the same rule.

A layout that reflows and loses the focused item is a layout that broke the user's task. The Skill rejects "structural change silently destroys task context" as an anti-pattern.

## Pillar 1: Focus preservation

### Definition

When a structural change occurs (e.g. sidebar collapses; multi-column becomes single-column), the element that had keyboard focus before the change remains focused after the change.

### Apple platforms

- SwiftUI `@FocusState` is preserved across structural change if the focus binding is on a stable identifier, not on the view's position. Example: `@FocusState private var focusedItem: Item.ID?` is preserved; `@FocusState private var focusedItem: Item?` (positional) is NOT.
- UIKit `UIResponder` chain is preserved across `UISplitViewController` collapse. The system handles it.
- Web: `document.activeElement` is preserved across layout reflow IF the element is not removed from the DOM. The Skill does NOT remove the focused element from the DOM during a reflow.

### Web implementation

```javascript
function reflowLayout() {
  const active = document.activeElement;
  const activeId = active?.id;
  // ... perform reflow ...
  if (activeId) {
    const restored = document.getElementById(activeId);
    if (restored) {
      restored.focus({ preventScroll: true });
    }
  }
}
```

### Anti-patterns

- **`element.blur()` during reflow** — explicitly blurring the focused element. Rejected. The Skill preserves focus.
- **Focus on a position, not an ID** — `@FocusState` bound to a view's position. Rejected. The Skill binds focus to a stable identifier.
- **Focus lost when the element is removed from the DOM** — the element is removed; focus is lost. Rejected. The Skill does NOT remove the focused element from the DOM during reflow.

## Pillar 2: Selection preservation

### Definition

When a structural change occurs, the selected element(s) remain selected across the change.

### Apple platforms

- SwiftUI `@Selection` is preserved across structural change if the selection binding is on a stable identifier. The Skill does NOT re-create the selection state on reflow.
- UIKit `UICollectionView.indexPathsForSelectedItems` is preserved across `UICollectionViewLayout` changes. The system handles it.
- Web: `Selection` API is preserved across layout reflow IF the selection is on a text node that is not removed from the DOM. The Skill does NOT remove the selected text node during a reflow.

### Web implementation

```javascript
function reflowLayout() {
  const selection = window.getSelection();
  if (!selection || selection.rangeCount === 0) return;
  const range = selection.getRangeAt(0);
  const startContainerId = range.startContainer.parentElement?.id;
  const startOffset = range.startOffset;
  // ... perform reflow ...
  if (startContainerId) {
    const container = document.getElementById(startContainerId);
    if (container) {
      const newRange = document.createRange();
      newRange.setStart(container.firstChild, Math.min(startOffset, container.firstChild.length));
      newRange.collapse(true);
      selection.removeAllRanges();
      selection.addRange(newRange);
    }
  }
}
```

### Anti-patterns

- **Selection cleared during reflow** — `window.getSelection().removeAllRanges()` during reflow. Rejected. The Skill preserves selection.
- **Re-creating the selection state on reflow** — re-running the selection logic. Rejected. The Skill preserves the existing selection.

## Pillar 3: Scroll position preservation

### Definition

When a structural change occurs, the scroll position of each surface is preserved (or restored) across the change. The scroll position is a property of the surface, not of the page.

### Apple platforms

- SwiftUI `ScrollView` preserves its `scrollPosition` binding across structural change.
- UIKit `UIScrollView.contentOffset` is preserved across `UIScrollView` frame changes.
- Web: `element.scrollTop` / `element.scrollLeft` is preserved across layout reflow IF the element is not removed from the DOM.

### Web implementation

```javascript
function reflowLayout() {
  const surfaces = document.querySelectorAll('[data-preserve-scroll]');
  const saved = Array.from(surfaces).map((el) => ({
    id: el.id,
    top: el.scrollTop,
    left: el.scrollLeft,
  }));
  // ... perform reflow ...
  saved.forEach(({ id, top, left }) => {
    const el = document.getElementById(id);
    if (el) {
      el.scrollTop = top;
      el.scrollLeft = left;
    }
  });
}
```

### Anti-patterns

- **Scroll position lost during reflow** — the reflow resets `scrollTop` to 0. Rejected. The Skill preserves scroll position.
- **Page-level scroll only** — only the document's scroll is preserved; nested surfaces' scroll is reset. Rejected. The Skill preserves scroll per surface.
- **Scroll restoration only on browser back** — `history.scrollRestoration = 'auto'` is the only scroll preservation. Rejected. The Skill preserves scroll on every reflow.

## Pillar 4: Back stack integrity

### Definition

When a structural change occurs, the navigation back stack is intact. Collapsing a sidebar does not clear the drill-in stack. Closing a multi-column layout does not pop the navigation stack.

### Apple platforms

- SwiftUI `NavigationStack` path is preserved across `NavigationSplitView` collapse. The system handles it.
- UIKit `UINavigationController.viewControllers` is preserved across `UISplitViewController` collapse.
- Web: `history.state` is preserved across layout reflow. The browser handles it; the Skill does NOT call `history.replaceState()` on reflow.

### Anti-patterns

- **`history.replaceState()` on reflow** — replacing the history entry on every reflow. Rejected. The Skill does NOT touch history on reflow.
- **Clearing the navigation stack on sidebar collapse** — `navigationStack.path = []` on reflow. Rejected. The Skill preserves the path.

## Reversibility: the user can override

The Skill publishes a `no forced anatomy` rule: if the platform supports the user's preferred anatomy at this width, the Skill does NOT override. The Skill exposes the override affordance.

### Examples

- **Always sidebar** — the user can pin a sidebar open even at a width that would normally collapse it. SwiftUI: `NavigationSplitView(columnVisibility: .constant(.all))`. Web: a manual toggle.
- **Always compact** — the user can pin the compact form even at a width that would normally expand to multi-column. SwiftUI: a `@State` that overrides the size class. Web: a CSS class that overrides the media query.
- **Auto / manual split** — the user can choose between auto-collapse and manual-collapse. SwiftUI: `columnVisibility: .automatic` vs `.constant`. Web: a media query paired with a manual override.

### Implementation

The Skill exposes the override as a first-class affordance. The override is a state variable, not a side effect of layout. The Skill does NOT silently override the user's preference.

### Anti-patterns

- **No override** — the layout is fully automatic; the user cannot pin a choice. Rejected where the platform supports an override.
- **Override hidden in settings** — the user has to dig through 4 levels of settings to find the override. Rejected. The Skill exposes the override as a first-class affordance (e.g. a toolbar button).
- **Override lost on restart** — the user's pinned choice is forgotten on the next launch. Rejected. The Skill persists the override.

## What the Skill does NOT do

- Does NOT `blur()` the focused element during reflow.
- Does NOT clear the selection during reflow.
- Does NOT reset the scroll position during reflow.
- Does NOT clear the back stack during reflow.
- Does NOT force a layout; the user can override.
- Does NOT silently destroy task context. The Skill is explicit about what is preserved and what is not.
