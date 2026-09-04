# Web Structural Primitives

The canonical Web primitives for adaptive structure. The Skill uses these primitives for layout, multi-column, container-level adaptivity, transitions, popovers, and anchor positioning. The Skill does NOT recommend JavaScript for layout work that these primitives can do declaratively.

## CSS Container Queries (component-level adaptivity)

### Why

Viewport media queries (`@media (min-width: 600px)`) adapt the layout to the viewport, not the component. Container queries adapt the layout to the component's containing block. The Skill uses container queries for component-level adaptivity and viewport media queries for page-level adaptivity.

### Canonical usage

```css
.card-grid {
  container-type: inline-size;
  container-name: card-grid;
}

@container card-grid (min-width: 600px) {
  .card-grid__items {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}

@container card-grid (min-width: 900px) {
  .card-grid__items {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

### Container type options

- `container-type: normal` — the default. The element is not a container.
- `container-type: inline-size` — the element is a container for the inline (horizontal) size. Cheap. The Skill's default.
- `container-type: size` — the element is a container for both dimensions. More expensive. The Skill recommends this only when the component needs to adapt to both width and height.

### Container name

`container-name` is a convenience for `@container <name>`. The Skill uses named containers when the same component appears in multiple contexts with different breakpoints.

### Container query units

- `cqw` — 1% of the container's width.
- `cqh` — 1% of the container's height.
- `cqi` — 1% of the container's inline size.
- `cqb` — 1% of the container's block size.

The Skill uses `cqi` for the inline axis. The Skill does NOT use container query units for fixed dimensions; the Skill uses them for proportional sizing.

### Anti-patterns

- **Containerless component adaptivity** — components that adapt only via viewport media queries. Rejected. Container queries are the canonical component-level adaptivity.
- **Expensive `container-type: size`** — using `size` when `inline-size` is sufficient. Rejected on performance grounds.
- **Magic-number container queries** — `@container (min-width: 743px)` (a device-specific number). Rejected; the Skill uses round, documentable widths.
- **Nested container queries without naming** — `@container (min-width: 600px)` inside a context where the container is ambiguous. Rejected. The Skill uses `container-name`.

## CSS Grid (multi-column / responsive grid)

### Why

CSS Grid is the canonical Web primitive for 2-D layout. The Skill uses Grid for multi-column anatomy. The Skill does NOT use `display: flex` for primary 2-D layout.

### Canonical usage

```css
.layout {
  display: grid;
  grid-template-columns: 240px 1fr 280px;
  grid-template-rows: auto 1fr auto;
  gap: 16px;
}

@container (max-width: 840px) {
  .layout {
    grid-template-columns: 1fr;
    grid-template-areas: "sidebar" "content" "inspector";
  }
}
```

### Auto-fit / auto-fill

`repeat(auto-fit, minmax(240px, 1fr))` adapts the column count to the container's width. The Skill uses this for card grids.

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 16px;
}
```

### Subgrid

`grid-template-columns: subgrid` allows a child grid to inherit the parent grid's tracks. The Skill uses Subgrid for nested alignment.

### Anti-patterns

- **Flexbox for 2-D layout** — using `display: flex` with `flex-wrap: wrap` for multi-column. Rejected on clarity grounds; the Skill uses Grid.
- **Fixed column widths** — `grid-template-columns: 320px 1024px 320px` as a structural anchor. Rejected. Geometry is fluid.
- **12-column canonization** — `grid-template-columns: repeat(12, 1fr)` as the only grid. Rejected as a canonical rule. 12-column is a HEURISTIC; the Skill allows 4, 8, 16, 24 column grids.

## View Transitions API (in-page animated transitions)

### Why

The View Transitions API is the canonical Web primitive for in-page animated transitions, including across size-class changes. The Skill uses the View Transitions API for cross-anatomy transitions; where unsupported, the Skill falls back to CSS transitions on `transform` / `opacity`.

### Canonical usage

```javascript
function navigateTo(newUrl) {
  if (!document.startViewTransition) {
    // Fallback: no animation
    location.href = newUrl;
    return;
  }
  document.startViewTransition(() => {
    // Update the DOM to the new state
    return new Promise((resolve) => {
      // ... update DOM ...
      resolve();
    });
  });
}
```

```css
::view-transition-old(root) {
  animation: fade-out 200ms ease-out;
}

::view-transition-new(root) {
  animation: fade-in 200ms ease-in;
}
```

### View Transitions Level 2 (cross-document)

The View Transitions API Level 2 supports cross-document transitions. The Skill records this as a forward-looking primitive.

### Anti-patterns

- **Heavy transitions across size-class changes** — full-page slide-in on every resize. Rejected on performance grounds. The Skill restricts transitions to `transform` / `opacity` / `clip-path` only.
- **View Transitions without `prefers-reduced-motion`** — full animation regardless of user preference. Rejected. The Skill honors `@media (prefers-reduced-motion: reduce)`.
- **JavaScript-only transitions** — `setTimeout` + `transform` instead of the View Transitions API. Rejected where the API is supported. The Skill routes to the canonical primitive.

## Popover API (non-modal auxiliary surfaces)

### Why

The Popover API is the canonical Web primitive for non-modal auxiliary surfaces with light-dismiss. The Skill uses the Popover API for inspector, secondary detail, and other on-demand surfaces.

### Canonical usage

```html
<button popovertarget="inspector">Toggle Inspector</button>
<div id="inspector" popover="manual">...</div>
```

```css
#inspector {
  /* Anchor to the button via CSS Anchor Positioning */
  position-anchor: --button-anchor;
  position-area: right span-block-end;
  margin: 0;
}
```

```css
[popovertarget] {
  anchor-name: --button-anchor;
}
```

### `popover="auto"` vs `popover="manual"`

- `auto` — light-dismiss: clicking outside the popover closes it. The default.
- `manual` — the popover does not light-dismiss. The Skill uses `manual` when the popover is a non-modal inspector that should stay open while the user works.

### Anti-patterns

- **JavaScript-managed popover** — manual show / hide via JS. Rejected where the Popover API is supported. The Skill routes to the canonical primitive.
- **Modal popover** — using `popover="auto"` for a modal surface. Rejected. The Popover API is non-modal; for modal surfaces, the Skill uses the `<dialog>` element with `showModal()`.
- **Popover without light-dismiss** — `popover="auto"` that ignores outside clicks. Rejected. The Skill honors light-dismiss.

## CSS Anchor Positioning (forward-looking)

### Why

CSS Anchor Positioning allows a popover, tooltip, or menu to be anchored to a target element without JavaScript. The Skill uses this as a forward-looking primitive.

### Canonical usage

```css
.button {
  anchor-name: --button;
}

.popover {
  position: fixed;
  position-anchor: --button;
  position-area: top span-inline-end;
  margin: 0;
}

@supports (anchor-name: --test) {
  /* Anchor Positioning is supported */
}
```

### Browser support

Anchor Positioning is in active development. The Skill records it as a forward-looking primitive; for current support, the Skill uses absolute positioning relative to a manually-positioned `anchor` element via polyfill.

### Anti-patterns

- **Anchor Positioning without `@supports`** — using the feature without a fallback. Rejected. The Skill pairs Anchor Positioning with `@supports` and a fallback.
- **JavaScript-only anchoring** — using `getBoundingClientRect` + manual positioning instead of Anchor Positioning. Rejected where supported.

## Reduced motion / reduced transparency / Dynamic Type

The Skill honors the canonical Web accessibility media queries:

- `@media (prefers-reduced-motion: reduce)` — reduce or remove animations.
- `@media (prefers-reduced-transparency: reduce)` — use more opaque surfaces.
- `@media (prefers-contrast: more)` — strengthen contrast.
- `@media (prefers-color-scheme: dark)` — dark mode.

The Skill also honors Dynamic Type on Web via `rem` / `em` units and `font-size: clamp()`. The Skill does NOT pin text to `px` units.

## What the Skill does NOT do

- Does NOT use `display: flex` for primary 2-D layout. The Skill uses Grid.
- Does NOT use JavaScript for layout work that CSS can do declaratively.
- Does NOT use fixed geometry. Geometry is fluid.
- Does NOT use viewport media queries alone. The Skill pairs them with container queries.
- Does NOT invent Web primitives. The Skill uses the canonical W3C / WHATWG / browser-supported primitives.
- Does NOT re-implement a popover in JS when the Popover API is supported.
- Does NOT use `prefers-reduced-motion` as the only way to detect reduced motion; the Skill also respects user settings at the OS level.
