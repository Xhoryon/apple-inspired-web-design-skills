# Apple-inspired Web Design Skills Pack

A pack of seven Skills that teach a future agent to build and audit web interfaces in the Apple visual language — typography, spacing, geometry, restrained color, motion, components, and a real-browser audit loop. The pack also defines a Web approximation of Apple Liquid Glass and the rules for when (and when not) to use it.

**Status:** v1.0 — Stable baseline
**Independent gate:** PASS WITH NOTED LIMITATIONS
**License:** [PolyForm Noncommercial 1.0.0](./LICENSE)

## Non-affiliation

This project is **Apple-inspired but independent**. It is not affiliated with, endorsed by, or sponsored by Apple Inc. It does not provide an official Apple Liquid Glass Web implementation. Apple, MacBook, iPhone, iPad, Pro, Max, Ultra, visionOS, Liquid Glass, and SF Pro are trademarks of Apple Inc.

## Core principles

1. **Liquid Glass is not glassmorphism.** A `backdrop-filter: blur()` + white fill + 1 px border is the baseline glass material. Liquid Glass requires ≥ 3 of 7 observable dimensions (translucency, refraction, specular, adaptivity, depth, dynamics, morphing).
2. **Glass is an interaction-layer material.** It belongs on floating controls (navbar, toolbar, sheet, popover, segmented control, media overlay), not on body text, tables, forms, or every card.
3. **Apple-inspired is a quality bar, not a brand replacement.** The pack teaches hierarchy, restraint, motion, and material discipline. The user's brand (color, voice, identity) stays.
4. **Mobile is a redesign, not a shrink.** Mobile footers, nav, hero, and CTA sizing are recomposed — not auto-scaled.
5. **Typography carries hierarchy.** Off-white backgrounds, near-black text, negative tracking on display only, modular spacing, single restrained accent color.
6. **Accessibility and performance are first-class.** `prefers-reduced-motion`, `prefers-reduced-transparency`, `prefers-contrast`, focus rings, 44×44 hit targets, GPU budget on glass surfaces.

## What's included

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

Detailed recipes in each skill's `references/` directory.

### Anti-pattern library (27)

Explicit "do not do this" entries in `apple-design-audit/references/anti-patterns.md`. Each entry has symptom / why-it-fails / detection / correction.

## Liquid Glass distinction

The pack enforces three terms as technically distinct:

- **Frosted Glass:** `backdrop-filter: blur()` + translucent fill. Building block.
- **Glassmorphism:** Frosted Glass + 24 px radius + 1 px white border + colored backdrop. Visual genre.
- **Liquid Glass:** Apple's 2025+ material — a 7-dimension behavior, not a single visual effect. The Web approximation uses SVG `feDisplacementMap` for refraction (Level 2) or WebGL/WebGPU shader (Level 3).

Web has no native `glassEffect()` equivalent of SwiftUI. The pack is honest about this gap: Web "Liquid Glass" is a high-quality approximation, not a port.

## Installation

Copy the desired skill directories into your agent's skills directory. Example with `~/.agents/skills/`:

```
cp -R skills/apple-* ~/.agents/skills/
```

After copying, your agent will discover the Skills by name when a relevant request matches the frontmatter description.

## Agent compatibility

The same seven Skill directories are the canonical source for every supported agent; do not maintain agent-specific copies.

- **Codex / Agent Skills:** install into the Skills directory supported by the runtime.
- **DeepSeek Harness:** `~/.agents/skills/` or `~/.dsh/skills/`.
- **Claude Code:** `~/.claude/skills/`.
- **Other Agent Skills-compatible agents:** copy the seven directories into the runtime's configured Skills directory.

Paths may be configurable; confirm the active runtime's documentation or configuration.

## Validation status

v1.0 was validated during development with five original adversarial scenarios, six regression scenarios, Playwright Chromium checks, mobile/CJK/touch-target/reduced-motion coverage, and Liquid Glass optical comparisons. The internal harness produced 25 screenshots across the validation matrix.

**Validation artifacts are maintained internally and are not included in the public Skills distribution.**

## Known limitations

Documented in the independent gate verdict and not blockers:

1. **Chromium independently verified only.** Playwright chromium-1228 was the test target. Safari, Firefox, and Edge are not independently browser-validated in v1.0; the design intent is portable across them per spec, but not proven.
2. **No Lighthouse run.** Performance received browser-level sanity checks during development; no simulated score was claimed.
3. **No real low-end-device profiling.** Mid-tier Android profile was not exercised.
4. **No Level 3 (WebGL/WebGPU) shader demonstration in the browser.** Level 3 is documented in `apple-liquid-glass-web/references/shader-glass.md`; v1.0 validation stopped at Level 2 SVG distortion.
5. **Only 2× DPR tested.** 3× DPR (iPhone 12+) was not validated.

## Maintenance freeze

v1.0 is frozen. Future modifications enter v1.1 only when:

1. A real project exposes a recurring design failure current rules don't address.
2. A new browser/API changes Web Liquid Glass implementation capability.
3. Apple Design Language ships a substantive update.
4. Multiple real projects prove a gap in current rules.
5. Reference discovery / token cost requires restructure.

The pack does not expand for theoretical completeness.

## License

PolyForm Noncommercial License 1.0.0 — see [LICENSE](./LICENSE).

- Noncommercial use is allowed.
- Modification and redistribution permitted subject to license terms.
- Commercial use requires separate written permission.

## See also

- [RELEASE-NOTES-v1.0.md](./RELEASE-NOTES-v1.0.md) — public release summary.
