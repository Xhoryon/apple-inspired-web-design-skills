# Apple-inspired Web Design Skills Pack — v1.0

**Status:** v1.0 — Stable baseline
**Independent gate:** PASS WITH NOTED LIMITATIONS
**License:** PolyForm Noncommercial License 1.0.0

## What is this?

A pack of seven Skills that teach a future agent to build and audit web interfaces in the Apple visual language: typography, spacing, geometry, restrained color, motion, components, and a real-browser audit loop. The pack also defines a Web approximation of Apple Liquid Glass and the rules for when (and when not) to use it.

## What's in v1.0

### Skills (7)

| Skill | Purpose |
|---|---|
| `apple-web-design` | Entry / router. Decides which sub-skills apply. |
| `apple-design-foundations` | Typography, spacing, geometry, color, depth — the base layer. |
| `apple-web-composition` | Hero archetypes, section vocabulary, narrative pacing. |
| `apple-motion-interaction` | Spring curves, continuity, spatial transitions, reduced motion. |
| `apple-ui-components` | Role-based component grammar (when to use what). |
| `apple-liquid-glass-web` | Liquid Glass as a 7-dimension material with Web implementation levels 0–3. |
| `apple-design-audit` | Mandatory browser-screenshot audit + 100-point checklist. |

### References (24)

Detailed recipes in each skill's `references/` directory. Cross-skill references in `apple-web-design/references/pack-overview.md`.

### Anti-pattern library (27)

Explicit "do not do this" entries in `apple-design-audit/references/anti-patterns.md`. Each has symptom / why-it-fails / detection / correction.

## Core principles

These are the things the pack will refuse to ship without:

1. **Liquid Glass is not glassmorphism.** A `backdrop-filter: blur()` + white fill + 1 px border is the *baseline* glass material. Liquid Glass requires ≥ 3 of 7 observable dimensions (translucency, refraction, specular, adaptivity, depth, dynamics, morphing).
2. **Glass is an interaction-layer material.** It belongs on floating controls (navbar, toolbar, sheet, popover, segmented control, media overlay), not on body text, tables, forms, or every card.
3. **Apple-inspired is a quality bar, not a brand replacement.** The pack teaches hierarchy, restraint, motion, and material discipline. The user's brand (color, voice, identity) stays.
4. **Mobile is a redesign, not a shrink.** Mobile footers, nav, hero, and CTA sizing are recomposed — not auto-scaled.
5. **Typography carries hierarchy.** Off-white backgrounds, near-black text, negative tracking on display only, modular spacing, single restrained accent color.
6. **Accessibility and performance are first-class.** `prefers-reduced-motion`, `prefers-reduced-transparency`, `prefers-contrast`, focus rings, 44×44 hit targets, GPU budget on glass surfaces.

## Liquid Glass distinction

The pack enforces three terms as technically distinct:

- **Frosted Glass:** `backdrop-filter: blur()` + translucent fill. Building block.
- **Glassmorphism:** Frosted Glass + 24 px radius + 1 px white border + colored backdrop. Visual genre.
- **Liquid Glass:** Apple's 2025+ material — a 7-dimension behavior, not a single visual effect. The Web approximation uses SVG `feDisplacementMap` for refraction (Level 2) or WebGL/WebGPU shader (Level 3).

Web has no native `glassEffect()` equivalent of SwiftUI. The pack is honest about this gap: Web "Liquid Glass" is a high-quality approximation, not a port.

## Validation performed during v1.0 development

v1.0 was validated with:

- Five original adversarial scenarios (A–E).
- Six regression scenarios (T1–T6), including browser-verified CJK and brand-preservation cases.
- Playwright Chromium checks covering mobile composition, touch targets, reduced motion, and Liquid Glass optical behavior.
- An internal screenshot matrix of 25 captures.

The browser harness, HTML pages, capture tooling, audit data, and screenshots are maintained internally and are not included in the public Skills distribution.

## Known limitations (carried in v1.0)

These are documented in the independent gate verdict and not blockers:

1. **Chromium independently verified only.** Playwright chromium-1228 was the test target. Safari, Firefox, and Edge are not independently browser-validated in v1.0; the design intent is portable across them per spec, but not proven.
2. **No Lighthouse run.** Performance received browser-level sanity checks during development; no simulated score was claimed.
3. **No real low-end-device profiling.** Mid-tier Android profile was not exercised.
4. **No Level 3 (WebGL/WebGPU) shader demonstration in the browser.** Level 3 is documented in `apple-liquid-glass-web/references/shader-glass.md`; v1.0 validation stopped at Level 2 SVG distortion.
5. **Only 2× DPR tested.** 3× DPR (iPhone 12+) was not validated.

## Maintenance freeze

v1.0 is **frozen**. Future modifications enter v1.1 only when:

1. A real project exposes a recurring design failure current rules don't address.
2. A new browser/API changes Web Liquid Glass implementation capability.
3. Apple Design Language ships a substantive update.
4. Multiple real projects prove a gap in current rules.
5. Reference discovery / token cost requires restructure.

The pack does not expand for theoretical completeness.

## Installation

Copy the desired skill directories into your agent's skills directory. Example with `~/.agents/skills/`:

```
cp -R skills/apple-* ~/.agents/skills/
```

After copying, your agent will discover the Skills by name when a relevant request matches the frontmatter description.

## License

PolyForm Noncommercial License 1.0.0.

- Noncommercial use is allowed.
- Modification and redistribution permitted subject to license terms.
- Commercial use requires separate written permission.

See `LICENSE` for the full text.

## Non-affiliation

This project is **Apple-inspired but independent**.

- Not affiliated with, endorsed by, or sponsored by Apple Inc.
- Does not provide an official Apple Liquid Glass Web implementation.
- Apple, MacBook, iPhone, iPad, Pro, Max, Ultra, visionOS, and Liquid Glass are trademarks of Apple Inc.

The pack teaches principles; the user's brand stays the user's.
