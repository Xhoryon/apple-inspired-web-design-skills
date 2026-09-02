# Apple-inspired Web Design Skills Pack

Seven reusable Agent Skills for applying Apple-inspired design judgment to Web interfaces. The pack covers foundations, composition, motion, component grammar, Liquid Glass, and design auditing; it is a decision system rather than a template collection.

**Status:** v1.1.0 — Stable baseline

**License:** [PolyForm Noncommercial 1.0.0](./LICENSE)

## Non-affiliation

This project is Apple-inspired but independent. It is not affiliated with, endorsed by, or sponsored by Apple Inc. It does not provide an official Apple Liquid Glass Web API or a native-equivalent implementation, and it does not distribute Apple trademark assets.

## What changed in v1.1

v1.1 is an architecture and precision release. It improves when Skills trigger, which companion Skills load, how supporting references are disclosed, and how audits work with different agent capabilities.

### Smarter routing

The `apple-web-design` router now classifies requests by task family and marks companion Skills as:

- **Required** — load for this task.
- **Conditional** — load only when the named condition applies.
- **Usually skip** — avoid for focused work.

Small tasks no longer need the full pack. v1.1 reduces unnecessary companion-skill loading through task-aware routing.

### Universal Audit

`apple-design-audit` now has two capability-gated paths:

- **Universal Audit:** works from available evidence such as text, source, tokens, or supplied screenshots. It does not require a browser.
- **Browser / Runtime Verification:** an optional second path for browser-capable agents. It adds rendered evidence, responsive checks, computed styles, and runtime behavior.

For final visual QA, browser-capable agents should still inspect rendered evidence when appropriate. Reports must disclose which path and evidence were used.

### Brand-aware foundations

Existing brand tokens take precedence over Apple-leaning defaults. Apple-inspired quality means applying restraint, hierarchy, material discipline, and accessibility within the product's identity—not replacing that identity. Apple palette values in the pack are starting heuristics, not mandatory tokens.

### Broader composition and CJK guidance

Composition guidance now explicitly covers:

- marketing and product pages;
- dense productivity and data interfaces;
- editorial and reading experiences.

CJK awareness remains part of the pack. Numeric typography values are starting heuristics and require validation against the actual font, script, brand, and composition; PingFang-derived values are not universal East Asian typography rules.

## Skills

| Skill | Purpose |
|---|---|
| `apple-web-design` | Entry router with task-aware Skill selection. |
| `apple-design-foundations` | Typography, spacing, geometry, color, depth, and brand precedence. |
| `apple-web-composition` | Marketing, product, dense application, and editorial composition. |
| `apple-motion-interaction` | State continuity, spring behavior, feedback, and reduced motion. |
| `apple-ui-components` | Role-based component grammar and interaction hierarchy. |
| `apple-liquid-glass-web` | Liquid Glass principles and Web implementation Levels 0–3. |
| `apple-design-audit` | Universal design audit plus optional browser/runtime verification. |

The public package contains **7 Skills**, **26 references**, and **26 anti-patterns**.

## Core principles

1. **Content first.** Hierarchy and clarity lead; effects support them.
2. **Apple-inspired is not an Apple clone.** Preserve the product's identity and platform appropriateness.
3. **Liquid Glass is not blur or glassmorphism.** Basic frosted glass is only a building block.
4. **Glass belongs primarily to the interaction layer.** Use it for controls and floating surfaces, not every content container.
5. **Mobile is a composition redesign, not a desktop shrink.**
6. **Accessibility and performance are first-class.** Reduced motion/transparency, contrast, focus, 44×44 touch targets, and rendering cost matter.

## Liquid Glass on the Web

The pack distinguishes:

- **Frosted Glass:** blur plus a translucent surface.
- **Glassmorphism:** a visual genre built from frosted surfaces, borders, radii, and colored backdrops.
- **Liquid Glass:** a behavior model spanning translucency, refraction, specular response, adaptivity, depth, dynamics, and morphing.

Web implementations are approximations. The pack defines progressive Levels 0–3, from solid fallback through CSS, SVG-enhanced, and high-complexity shader approaches. Internal browser validation currently reaches Level 2 SVG displacement; Level 3 is documented but is not the primary validated default or a native-equivalent result.

## Installation

Copy all Skills, or only the task-specific directories you need, into the Skills directory supported by your agent runtime. For example:

```sh
cp -R skills/apple-* ~/.agents/skills/
```

Keep the seven directories together when you want the router to coordinate the complete pack. Runtime-specific paths may be configurable; use the active agent's documentation or configuration as the source of truth.

## Agent compatibility

The canonical Skill source is provider-neutral:

- `name`, `description`, `version`, and `license` frontmatter;
- Markdown bodies;
- relative references.

Static compatibility was checked for Codex, Claude Code, and DeepSeek Harness. **Runtime cross-agent invocation was not independently executed for v1.1.** The project maintains one canonical Skill source rather than provider-specific forks.

## Validation

During v1.1 development:

- 17 regression scenarios passed;
- 12 core invariants were preserved;
- routing, brand preservation, dense/editorial composition, CJK handling, capability-gated auditing, accessibility, and Liquid Glass safeguards were checked.

Validation artifacts are maintained internally and are not included in the public Skills distribution.

## Known limitations

- Runtime invocation across Codex, Claude Code, and DeepSeek Harness was not independently executed for v1.1.
- Web Liquid Glass remains an approximation of publicly described design principles, not native Apple material behavior.
- Level 3 shader glass remains a documented high-complexity option, not the primary validated default.
- Broad cross-browser and device validation remains limited relative to native Apple platforms.
- CJK numeric guidance is heuristic and font-, script-, brand-, and context-dependent.

Future development may broaden interaction and motion fidelity, but those capabilities are not part of v1.1.

## License

PolyForm Noncommercial License 1.0.0 — see [LICENSE](./LICENSE).

- Noncommercial use is permitted under the license terms.
- Modification and redistribution are permitted under the license terms.
- Commercial use requires separate permission.

## Release notes

- [v1.1](./RELEASE-NOTES-v1.1.md)
- [v1.0](./RELEASE-NOTES-v1.0.md)
