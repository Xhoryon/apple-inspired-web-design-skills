# Size Classes and Width

Apple HIG defines two size classes per dimension — `compact` and `regular` — exposed via `UITraitCollection.horizontalSizeClass` / `.verticalSizeClass` (UIKit) and `@Environment(\.horizontalSizeClass)` (SwiftUI). The Skill uses this vocabulary on Apple platforms and pairs it with a Web width ladder. (Paraphrase of current Apple HIG adaptive-design guidance; verified in DocC `UITraitCollection.md` + installed SDK.)

The Skill does NOT maintain a per-device model → size-class lookup table. The canonical source for current device size-class behavior is the live Apple HIG device table. The Skill reasons from the current trait collection (`horizontalSizeClass` / `verticalSizeClass` from `UITraitCollection`, or the SwiftUI `@Environment(\.horizontalSizeClass)` value), not from a model-name inference.

## Apple size class model (canonical)

### Horizontal size class

- **`compact`** — the horizontal dimension is constrained.
- **`regular`** — the horizontal dimension is wide.

### Vertical size class

- **`compact`** — the vertical dimension is constrained.
- **`regular`** — the vertical dimension is tall.

The values come from `UITraitCollection.horizontalSizeClass` / `verticalSizeClass` and the SwiftUI `horizontalSizeClass` / `verticalSizeClass` environment values. The constants are `UIUserInterfaceSizeClass.compact`, `.regular`, `.unspecified`. Default per form factor is documented in the Apple HIG device table; the Skill does NOT maintain a duplicate table here. Apps that need a stable reference should consult the live HIG.

### Common patterns (illustrative, not exhaustive)

- iPhone portrait: `compact-width, regular-height` in many current models; the HIG device table is authoritative.
- iPhone landscape: typically `compact-width, compact-height`, but several larger iPhone models report `regular-width, compact-height` in landscape. **Do not infer from device name or orientation alone** — read the trait at runtime.
- iPad portrait / landscape: `regular-width, regular-height` in full-screen. In Slide Over the iPad reports `compact-width, regular-height`.
- Mac Catalyst: inherits iPad size classes by default. An app can opt into Mac idiom via `UISceneSession` configuration.
- Mac (SwiftUI `Window`): `regular-width, regular-height` in standard window; SwiftUI `Window` can be resized down and traits update accordingly.
- visionOS: does not use the iOS / iPadOS size class model. Uses its own spatial layout (Window / Volume / Space).
- tvOS: focus-driven; size class not the primary driver.
- watchOS: stack-driven; size class not the primary driver.

The Skill does NOT treat Mac Catalyst as a separate structural family; it is iPad anatomy on Mac hardware.

## Web width class ladder (IMPLEMENTATION HEURISTIC)

The Skill publishes a width ladder as a Web implementation reference. The width ladder is a HEURISTIC; the Skill does NOT claim it is Apple canonical.

| Width range | Equivalent Apple size class | Recommended anatomy |
|---|---|---|
| 0–360 px | compact-width, regular-height | Card stack. Single column. No sidebar. |
| 361–600 px | compact-width, regular-height | Card stack with on-demand secondary surfaces. |
| 601–840 px | regular-width, regular-height (small) | 2-column max. Optional sidebar via on-demand. |
| 841–1024 px | regular-width, regular-height | 2- or 3-column. Sidebar visible. |
| 1025–1440 px | regular-width, regular-height | Full multi-column. Sidebar + content + optional inspector. |
| 1441+ px | regular-width, regular-height (large) | Multi-column with optional inspector. |

The width ladder is the Web equivalent of the size class model. The Skill does NOT claim that 600 px is the boundary; the boundary is the Web implementation choice. The size class vocabulary is preserved; the px numbers are HEURISTIC.

## Pairing the width class with the size class

The Skill pairs the Web width class with the Apple size class vocabulary in the source matrix and in the routing decisions. A task that asks for "compact width" is interpreted as "use the compact-width structural form" — on Apple, this means the system reports a compact size class; on Web, this means the width is in the compact ladder range.

The Skill does NOT publish numeric thresholds as canonical Apple values. The numeric thresholds are HEURISTIC and are documented as such.

## Why width over orientation

The available width and the current size class are the primary structural drivers. Orientation alone is not — `orientationchange` is a SECONDARY signal. A device in landscape can report either `compact-width` or `regular-width` depending on its model and the current traits. The Skill reasons from the current trait and available width, not from orientation.

## Container queries (canonical Web primitive for component-level adaptivity)

`@container` queries allow a component to adapt to the size of its containing block rather than the viewport. The Skill uses container queries for component-level adaptivity and viewport media queries for page-level adaptivity.

```css
.card {
  container-type: inline-size;
  container-name: card;
}

@container card (min-width: 480px) {
  .card__body {
    display: grid;
    grid-template-columns: 1fr 2fr;
  }
}
```

The Skill's rules:

- `container-type: inline-size` is the default for component-level adaptivity. Cheap.
- `container-type: size` (both dimensions) is more expensive; use only when the component needs to adapt to both width and height.
- Container queries are the canonical Web primitive. The Skill does NOT recommend JavaScript-driven adaptivity for component-level layout.

## Why width over orientation

The available width and the current size class are the primary structural drivers. Orientation alone is not — `orientationchange` is a SECONDARY signal. A device in landscape can report either `compact-width` or `regular-width` depending on its model and the current traits. The Skill reasons from the current trait and available width, not from orientation.

## Anti-patterns

- **Orientation as the driver** — `@media (orientation: landscape)` as the primary structural rule. Rejected. Width is the driver.
- **Width-class monoculture** — `@media (min-width: 600px)` alone. Rejected. The Skill pairs width with size class + window state + input context.
- **Containerless component adaptivity** — components that adapt only via viewport media queries. Rejected. Container queries are the canonical component-level adaptivity.
- **Fixed geometry** — `width: 1024px` as a structural anchor. Rejected. Geometry is fluid.
- **Magic numbers** — `min-width: 743px` (a number that happens to be a device-specific portrait width) used as a structural breakpoint. Rejected; the Skill does not canonize device-specific px values.
- **Size class as the only signal** — using only the size class without considering the input context, window state, or user preference. Rejected. The Skill pairs size class with the other modifiers.
