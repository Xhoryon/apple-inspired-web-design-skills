# Apple-inspired Web Design Skills Pack — v1.1

## Status

Stable release. v1.1 is an architecture and precision update built as a normal fast-forward from the frozen v1.0 baseline.

## What changed

v1.0 established the pack's design rules. v1.1 improves how agents discover and apply them:

- more precise Skill triggers and task-aware companion loading;
- progressive reference disclosure;
- universal and browser-capable audit paths;
- stronger brand precedence;
- broader dense and editorial composition guidance;
- provider-neutral canonical frontmatter;
- clearer CJK heuristic boundaries;
- a consolidated 26-entry anti-pattern library.

The release does not add a new family of Apple effects or claim native Apple behavior.

## Smarter routing

The `apple-web-design` router now maps task families to **Required**, **Conditional**, and **Usually skip** Skills. Focused component, audit, composition, CJK, brand, or Liquid Glass tasks can load only the material they need.

Tier-1 instructions grew to support explicit routing, while task-aware loading substantially reduces unnecessary downstream Skill context for focused tasks.

## Audit portability

`apple-design-audit` now separates:

- **Path A — Universal Audit:** applies the shared checklist and anti-pattern reasoning to available evidence without requiring a browser.
- **Path B — Browser / Runtime Verification:** adds rendered evidence when browser capabilities are available.

A browser-capable final QA pass should still inspect rendered output when appropriate. Audit reports must state which path ran and must not claim runtime facts that were not verified.

## Brand-aware foundations

Existing brand color, typography, voice, spacing, and geometry tokens take precedence. Apple-inspired quality is applied within the product's brand rather than replacing it with an Apple palette or template.

Apple-leaning palette and spacing values remain useful starting heuristics for projects without established tokens; they are not mandatory values.

## Broader composition

Composition guidance now explicitly supports:

- marketing and product storytelling;
- dense productivity, workspace, dashboard, and data surfaces;
- editorial and reading-oriented experiences.

These are defined task families, not a claim that every application archetype is covered.

## CJK guidance

CJK awareness remains a core invariant. Numeric tracking, weight, line-height, and inter-script spacing values are framed as starting heuristics that must be checked against the actual font, script, brand, and context. PingFang-derived observations are not treated as universal East Asian typography rules.

## Anti-pattern cleanup

The anti-pattern library now contains 26 entries. The consolidated set preserves critical safeguards including brand preservation, Apple Template Syndrome, interaction-layer discipline, CJK awareness, mobile touch targets, honest optical claims, and visible glass behavior.

## Progressive disclosure

Two new references support selective loading:

- `apple-web-design/references/by-topic.md`
- `apple-design-audit/references/browser-audit.md`

The canonical Skills remain usable as a single provider-neutral source with relative references.

## Validation

During v1.1 development:

- **17 / 17** regression scenarios passed.
- **12 / 12** core invariants were preserved.
- Static compatibility was checked for Codex, Claude Code, and DeepSeek Harness.
- Runtime cross-agent invocation was **not independently executed**.

Validation artifacts, scenarios, screenshots, scripts, and detailed audit reports are maintained internally and are not part of the public Skills distribution.

## Known limitations

- Runtime compatibility across Codex, Claude Code, and DeepSeek Harness was not independently executed for v1.1.
- Web Liquid Glass remains an approximation; it is not an official or native-equivalent Apple implementation.
- Level 3 shader glass remains documented and high-complexity, not the primary validated default.
- Cross-browser and device validation remains limited relative to native Apple behavior.
- CJK numeric guidance remains heuristic and font-dependent.

## Upgrade notes

- Replace installed v1.0 Skill directories with the seven v1.1 directories.
- Keep the directory structure intact so relative references continue to resolve.
- Existing v1.0 users should review the new task-to-Skill matrix and capability-gated audit paths.
- No test, browser-validation, compatibility-adapter, or audit-evidence directory is required for installation.

## License

PolyForm Noncommercial License 1.0.0 — see [LICENSE](./LICENSE).

Noncommercial use, modification, and redistribution are permitted under the license terms. Commercial use requires separate permission.

## Non-affiliation

This project is Apple-inspired but independent. It is not affiliated with, endorsed by, or sponsored by Apple Inc. It does not provide an official Apple Liquid Glass Web API or a native-equivalent implementation.
