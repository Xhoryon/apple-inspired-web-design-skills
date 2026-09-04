# Glass Groups Reference

A glass group is a set of glass surfaces that share a sampling region, a material identity, or a morphing relationship. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

Native Apple uses `GlassEffectContainer` (SwiftUI) and `UIGlassContainerEffect` (UIKit) to declare a shared sampling region across multiple glass surfaces. The Web has no direct equivalent; the Skill records the Web approximation honestly.

## The five group types

### 1. Independent glass

A single surface with no shared identity. The simplest case.

- **Native:** a single `.glassEffect(...)` modifier; the surface is its own `GlassEffectContainer`.
- **Web approximation:** a single DOM element with `backdrop-filter`.
- **When to use:** one-off surfaces with no siblings.

### 2. Coordinated glass cluster

Multiple glass surfaces that visually belong together. They may share a sampling region, an edge, or a context.

- **Native:** a `GlassEffectContainer` containing multiple `.glassEffect(...)` children. The shared container allows the surfaces to share a single sampling region and to morph / merge / retarget without seams.
- **Web approximation:** a single parent element whose children share the parent's `backdrop-filter` and optical effects. The implementer must verify visually that the children do not produce obviously inconsistent sampling.
- **When to use:** a toolbar with multiple glass controls; a segmented control with several glass segments; a command palette with a list of glass items.

The Skill does NOT claim the Web parent is equivalent to `GlassEffectContainer`. The Skill records the principle: when multiple glass surfaces belong together, the Web should treat them as a single coordinated cluster.

### 3. Shared highlight glass

A single highlight surface that retargets between sibling targets. Example: a segmented control with one selected pill that moves between segments.

- **Native:** a single highlight element within a `GlassEffectContainer`; the highlight uses `glassEffectID` to identify itself; transitions use `glassEffectTransition`.
- **Web approximation:** a single DOM element (a `<div>` with `backdrop-filter`) positioned absolutely inside the cluster. The element's `transform` and `width` are updated on selection change. The animation uses spring physics (HEURISTIC) and respects the retargeting rule from `apple-motion-physics`.
- **When to use:** segmented controls, tab bars with sliding indicator, scrollable carousels with a sliding highlight.

The shared highlight is a single material surface; it is NOT multiple surfaces that cross-fade. The native API achieves this with `glassEffectID`; the Web achieves it with a single absolutely positioned element.

### 4. Morphing group

A set of surfaces that can merge, separate, or reshape. Example: a floating toolbar that expands into a search surface; a small button that grows into a sheet.

- **Native:** `glassEffectTransition` with the appropriate morph identity.
- **Web approximation:** a single shared element that changes `transform`, `width`, `height`, `border-radius`, and `clip-path` between states. The element is a single material surface; the geometry interpolation is the morph.
- **When to use:** toolbar → search surface; small action → large action; compact → expanded; segmented control selection (which is also a shared highlight, depending on design).

The morphing group requires semantic continuity. A morphing group that morphs unrelated controls is rejected (Morph Without Meaning).

### 5. Proximity interaction

When two glass surfaces approach each other, they may share a sampling region. Example: a floating toolbar that gets closer to a tab bar as the user scrolls.

- **Native:** possible with overlapping `GlassEffectContainer`s; the system may merge them at the compositor level.
- **Web approximation:** heuristic. The implementer may consolidate two nearby glass surfaces into a single parent as the user scrolls; or may adjust the `backdrop-filter` to sample a wider region.
- **When to use:** rare; only when the design intent is to express that two surfaces belong together spatially.

Proximity interaction is a HEURISTIC on the Web. The implementer is not required to implement it.

## When surfaces should remain separate

Not every nearby glass surface belongs to the same group. The Skill rejects forced grouping when:

- The surfaces have different roles (e.g. a toolbar and a sheet are NOT the same group).
- The surfaces have different owners (e.g. a list item and a popover attached to it are NOT the same group; the popover is presented from the list item, not grouped with it).
- The grouping would force inconsistent sampling.

Forced grouping is a Web-only concern; native `GlassEffectContainer` declares intent explicitly.

## Sampling region

A glass surface samples the content directly behind it, not the whole page. When multiple glass surfaces share a sampling region, the visual effect is consistent.

The Web cannot directly reproduce this. The implementer must:

- Use a single parent element for the cluster.
- Ensure the parent's `backdrop-filter` samples the same region.
- Visually verify that the cluster reads as one material.
- If verification fails, consolidate further or accept the visual difference.

The Skill does NOT promise Web parity with `GlassEffectContainer`. The implementer is honest about the approximation.

## Glass group implementation patterns

### Segmented control (shared highlight)

```html
<div class="segmented" role="tablist" aria-label="View mode">
  <button role="tab" aria-selected="true" data-active>List</button>
  <button role="tab" aria-selected="false">Grid</button>
  <button role="tab" aria-selected="false">Map</button>
  <span class="indicator" aria-hidden="true"></span>
</div>
```

```css
.segmented {
  position: relative;
  display: inline-flex;
  background: rgba(0, 0, 0, 0.05);
  border-radius: 9999px;
  padding: 2px;
}
.segmented .indicator {
  position: absolute;
  top: 2px;
  left: 0;
  height: calc(100% - 4px);
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border-radius: 9999px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.10);
  transition: transform 280ms var(--ease-spring), width 280ms var(--ease-spring);
  pointer-events: none;
}
.segmented button {
  position: relative;
  z-index: 1;
  padding: 6px 14px;
  background: transparent;
  border: 0;
}
```

```js
// On click of a segment, animate the indicator to the new position.
const segs = document.querySelectorAll('.segmented button');
const indicator = document.querySelector('.segmented .indicator');
segs.forEach((seg) => {
  seg.addEventListener('click', () => {
    const rect = seg.getBoundingClientRect();
    const parent = seg.parentElement.getBoundingClientRect();
    indicator.style.transform = `translate(${rect.left - parent.left}px, 0)`;
    indicator.style.width = `${rect.width}px`;
  });
});
```

The indicator is a **single material surface**. It is not multiple cross-fading surfaces. This is the Web approximation of a shared highlight glass.

### Toolbar cluster (coordinated)

A toolbar with multiple glass controls. The implementer may:

- Use a single parent element for the toolbar with `backdrop-filter`.
- Place each child control inside the parent.
- Verify that the children share the parent's sampling region visually.

```html
<div class="glass-toolbar" role="toolbar" aria-label="Format">
  <button aria-label="Bold"><b>B</b></button>
  <button aria-label="Italic"><i>I</i></button>
  <button aria-label="Underline"><u>U</u></button>
</div>
```

```css
.glass-toolbar {
  position: fixed;
  inset: auto 12px 12px 12px;
  background: linear-gradient(180deg, rgba(255,255,255,0.55), rgba(255,255,255,0.35));
  border: 1px solid rgba(255,255,255,0.40);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border-radius: 22px;
  height: 56px;
  padding: 6px 8px;
  display: flex;
  align-items: center;
  gap: 4px;
}
.glass-toolbar button {
  background: transparent;
  border: 0;
  width: 44px;
  height: 44px;
  border-radius: 14px;
  /* No backdrop-filter on the child — the parent samples once. */
}
```

## No-mixing invariant (canonical)

Within a single glass group (a segmented control cluster, a toolbar cluster, a sheet, a popover), all surfaces share **one material variant family** (all Regular-like, or all Clear-like — never a mix).

This is the literal Apple rule: **Never mix Regular and Clear in the same interface / design treatment.** The Skill enforces it at the group level:

- A segmented control cluster is one Regular-like cluster. The selected pill is also Regular-like; selection is expressed through tint, foreground, border, geometry, or shared highlight — **not** through a Clear-like patch.
- A toolbar cluster is one Regular-like cluster.
- A popover is one Regular-like cluster.
- A sheet is one Regular-like cluster.
- A media overlay cluster (if any) is one Clear-like cluster.

The selection-state techniques within one material family (no variant mixing):

- **Tint** — a deliberate semantic overlay color (selected / primary / destructive / brand emphasis).
- **Foreground treatment** — text / icon color and weight change for the selected item.
- **Luminance treatment** — slightly brighter or darker fill on the selected item within the same variant.
- **Border / highlight** — a 1 px hairline border or an inset highlight on the selected item.
- **Shared geometry** — a single shared highlight surface (one element) that retargets between sibling targets.
- **Semantic selection state** — `aria-selected="true"` / `[data-selected]` etc.; the state is persistent, distinct from hover and focus.

These techniques preserve one material family within a cluster while expressing selection, focus, and hover. Mixing in a different variant family within the same cluster is rejected.

The variant is determined once for the whole group, then size / context / role inform fill, blur, edge, and selection techniques within that one family.

## What the Skill rejects

- Stacked full-page glass layers (glass-on-glass).
- Forced grouping of unrelated surfaces.
- Multiple cross-fading highlight surfaces (a single shared surface is the right model).
- Web-equivalent-claim of `GlassEffectContainer`. The Web is an approximation.
- **Mixing Regular-like and Clear-like within one group** (segmented control, toolbar cluster, sheet, popover, design treatment). Selection is expressed via tint / foreground / luminance / border / shared highlight / semantic state — **not** via variant-family mixing.

## Cross-Skill note

- The native `GlassEffectContainer` and `NSGlassEffectContainerView` are owned by the platform; the Skill recommends the system primitive. `UIGlassContainerEffect` is the iOS-family analog (`API_UNAVAILABLE(visionos, watchos)`); see `native-liquid-glass.md`.
- The shared highlight pattern is the Web approximation of `glassEffectID` + `glassEffectTransition(.matchedGeometry)`. The motion values are HEURISTIC owned by `apple-motion-physics`.
- The morph model is in `glass-morphing.md`.
- The material state model is in `glass-interaction.md`.
- The legibility model is `glass-legibility.md`.
- The no-mixing invariant for Regular / Clear is preserved in `SKILL.md` §"Regular vs Clear profile (Web approximation)" and `material-fidelity.md` §"Apple's three-condition rule for Clear" / §"Do not mix Regular-like and Clear-like within one design treatment". The Skill enforces it at the group level (segmented control cluster, toolbar cluster, sheet, popover).
