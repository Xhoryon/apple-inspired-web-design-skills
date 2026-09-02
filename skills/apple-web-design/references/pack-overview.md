# Apple-inspired Web Design Skills Pack — Overview

A one-page map of the whole pack. Read once, then dive into individual skills.

## Version

**v1.0 — Stable baseline (frozen).**

Independent Final Remediation Re-Gate: **PASS WITH NOTED LIMITATIONS**.

Documented limitations (not blockers): no Safari / Firefox / Edge independent verification, no Lighthouse run, no real low-end-device profiling, no Level 3 (WebGL/WebGPU) demonstration, no 3× DPR test, only 2× DPR. See [`../../../RELEASE-NOTES-v1.0.md`](../../../RELEASE-NOTES-v1.0.md) for the public release summary.

v1.1 changes require one of:
1. A real project exposes a recurring design failure.
2. New browser/API changes Web Liquid Glass implementation capability.
3. Apple Design Language ships a substantive update.
4. Multiple real projects prove a gap in current rules.
5. Reference discovery / token cost requires restructure.

## The pack

```
apple-web-design/                       (entry — routing)
├── SKILL.md                            routes to the other skills
└── references/pack-overview.md         this file

apple-design-foundations/               (always read first)
├── SKILL.md                            typography, space, geometry, color, depth
└── references/
    ├── typography.md
    ├── space-and-grid.md
    ├── geometry.md
    └── color-and-depth.md

apple-web-composition/                  (page-level)
├── SKILL.md                            hero, sections, narrative
└── references/
    ├── hero-patterns.md
    ├── section-vocabulary.md
    ├── scroll-storytelling.md
    └── mobile-composition.md

apple-motion-interaction/               (motion language)
├── SKILL.md                            spring, continuity, reduced motion
└── references/
    ├── spring-tokens.md
    └── reduced-motion.md

apple-ui-components/                    (parts)
├── SKILL.md                            role-based component grammar
└── references/
    ├── navigation.md
    ├── buttons.md
    ├── overlays.md
    └── forms.md

apple-liquid-glass-web/                 (most important — material)
├── SKILL.md                            definition + Web levels
└── references/
    ├── css-glass.md                    Level 0 / Level 1
    ├── svg-glass.md                    Level 2
    ├── shader-glass.md                 Level 3
    ├── contrast-on-glass.md
    └── performance.md

apple-design-audit/                     (mandatory final step)
├── SKILL.md                            browser QA, scoring, fix loop
└── references/
    ├── anti-patterns.md
    ├── screenshot-script.md
    └── perf-checklist.md
```

## Reading order

Always in this order. Skip a step only when the task explicitly excludes it.

1. **apple-design-foundations** — base layer. Typography, space, geometry, color, depth.
2. **apple-web-composition** — page-level. Hero, sections, narrative.
3. **apple-motion-interaction** — motion language.
4. **apple-ui-components** — component grammar.
5. **apple-liquid-glass-web** — only when the design needs an interaction-layer material.
6. **apple-design-audit** — always last. Browser QA, fix loop.

## Relationships

```
                              foundations
                                  |
                                  v
                            composition
                                  |
                                  v
                              motion
                                  |
                                  v
                            components
                                  |
                                  v
                            liquid-glass?  <-- optional
                                  |
                                  v
                              audit (always)
```

Each skill builds on the layer above it. The audit checks the whole stack.

## What this pack is NOT

- Not a 1:1 Apple.com clone kit. The pack teaches principles; the user's brand stays.
- Not a fixed component library. Skills describe roles and rules; the agent implements in the user's framework.
- Not a glassmorphism tutorial. Liquid Glass is more than blur; see `apple-liquid-glass-web`.
- Not a substitute for design judgment. Skills teach the rules; the agent still decides when to break them.

## When the pack fails

- The user wants to copy Apple.com pixel-for-pixel. The pack teaches principles, not pixel cloning.
- The user wants a Material / Fluent / other design language. Wrong pack.
- The user wants Apple logo, photography, or trademarked copy. Out of scope.

## Maintenance

This pack is the result of test-driven authoring. If a future agent finds a loophole where a skill is bypassed, fix the skill, not the agent. See `apple-design-audit/references/anti-patterns.md` for the current list of known failure modes.
