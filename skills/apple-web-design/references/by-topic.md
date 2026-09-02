# Reference Catalog by Topic (v1.1 — Tier 2)

This file is the **Tier 2 reference catalog** for the Apple-inspired Web Design Skills Pack. The router (`apple-web-design/SKILL.md`) does not list these inline; load this file only when you know which reference you need.

All paths below resolve from the skill root using the canonical public `skills/` subdir layout: from `apple-web-design/SKILL.md` the references sit at `apple-<skill>/references/<file>.md`. From the repository root they are at `skills/apple-<skill>/references/<file>.md`.

## Foundations (typography, spacing, geometry, color, depth, CJK)

| Topic | Reference |
|---|---|
| Type scale, font stack, hierarchy | `apple-design-foundations/references/typography.md` |
| Chinese / Japanese / Korean typography | `apple-design-foundations/references/cjk-typography.md` |
| Spacing scale, grid, gutters | `apple-design-foundations/references/space-and-grid.md` |
| Radius, concentric corners, optical alignment | `apple-design-foundations/references/geometry.md` |
| Color palette, accent, dark mode, depth | `apple-design-foundations/references/color-and-depth.md` |

## Composition (page-level, hero, sections, mobile)

| Topic | Reference |
|---|---|
| Hero archetype / first viewport | `apple-web-composition/references/hero-patterns.md` |
| Section variety on a long page | `apple-web-composition/references/section-vocabulary.md` |
| Pinned / scroll-driven storytelling | `apple-web-composition/references/scroll-storytelling.md` |
| Mobile redesign (not a shrink) | `apple-web-composition/references/mobile-composition.md` |

## Motion (spring, easing, accessibility)

| Topic | Reference |
|---|---|
| Spring curves, easing, animation tokens | `apple-motion-interaction/references/spring-tokens.md` |
| Reduced motion / reduced transparency | `apple-motion-interaction/references/reduced-motion.md` |

## UI Components (navigation, buttons, overlays, forms)

| Topic | Reference |
|---|---|
| Navbar / bottom tab bar / sidebar | `apple-ui-components/references/navigation.md` |
| Buttons (primary / secondary / tertiary / icon) | `apple-ui-components/references/buttons.md` |
| Sheet / popover / modal / drawer | `apple-ui-components/references/overlays.md` |
| Forms, inputs, validation | `apple-ui-components/references/forms.md` |

## Liquid Glass (Level 0–3)

| Topic | Reference |
|---|---|
| Level 1 CSS glass | `apple-liquid-glass-web/references/css-glass.md` |
| Level 2 SVG glass (displacement + specular) | `apple-liquid-glass-web/references/svg-glass.md` |
| Level 3 shader glass | `apple-liquid-glass-web/references/shader-glass.md` |
| Text contrast on glass | `apple-liquid-glass-web/references/contrast-on-glass.md` |
| Glass performance budget | `apple-liquid-glass-web/references/performance.md` |

## Audit (anti-patterns, screenshots, perf)

| Topic | Reference |
|---|---|
| Anti-pattern library (26 entries) | `apple-design-audit/references/anti-patterns.md` |
| Screenshot / capture script | `apple-design-audit/references/screenshot-script.md` |
| Browser execution adapter (browser audit) | `apple-design-audit/references/browser-audit.md` |
| Lighthouse / perf checklist | `apple-design-audit/references/perf-checklist.md` |

## How to use this catalog

1. Identify the topic you need (one of the rows above).
2. Load the corresponding reference file via its path.
3. If you need a *different* topic, return to this catalog rather than guessing paths.

The catalog is a **lookup index**, not a routing guide. For routing decisions (which Skills to load for which task), use `apple-web-design/SKILL.md` instead.