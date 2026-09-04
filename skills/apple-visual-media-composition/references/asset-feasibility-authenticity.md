# Asset Feasibility & Authenticity

## Media Asset Inventory

Before committing to advanced media behavior, inspect actual assets.

Possible inputs:
- JPEG
- PNG
- WebP
- AVIF
- transparent product render
- multiple views
- dense angular sequence
- video
- alpha video
- 3D model
- textures / materials
- screenshots
- diagrams
- illustrations
- existing WebGL scene

Classify each as one of:

- **AVAILABLE** — known to exist and inspected.
- **MISSING** — known not to exist.
- **UNKNOWN** — not yet inspected; may or may not exist.

Hard distinction: **UNKNOWN ≠ MISSING.** Ask before concluding the user does not possess an asset.

## Asset technical inspection

Where actual files exist, inspect:
- dimensions
- aspect ratio
- transparency
- orientation
- resolution
- color consistency where relevant
- frame continuity for sequences
- video dimensions / codec / duration
- 3D file type
- Web suitability

Do not assume: "I have a 3D file" means "ready for realtime WebGL."

## Asset Reality Before Visual Ambition

Major hard invariant: **Presentation ambition must not exceed the visual information the assets can reliably provide.**

Example: one front-view PNG cannot support authentic continuous 180° product rotation. CSS `rotateY` applied to a flat image is not a new physical viewpoint.

## Asset Capability Ladder (Skill vocabulary, NOT Apple canonical)

- **V0** — Single View. One trustworthy viewpoint.
- **V1** — Sparse Multi-View. Several discrete trustworthy viewpoints.
- **V2** — Dense Sequence. Sufficient continuous frames / angles.
- **V3** — Pre-rendered Cinematic. Video or fixed rendered timeline.
- **V4** — 3D Scene. Model with controllable viewpoint.
- **V5** — Interactive 3D Scene. Model + camera + appropriate runtime + interactive state.

Names may be refined. The ladder is the rule.

## Presentation Ambition (Skill vocabulary, NOT Apple canonical)

- **P0** — Static.
- **P1** — Composed Motion.
- **P2** — Scroll-linked Cinematic.
- **P3** — Multi-shot / Sequenced Cinematic.
- **P4** — Interactive Inspection.

Exact taxonomy may be refined. The important rule: **V capability and P ambition must be compatible.**

Example: V0 + continuous 360° rotation → infeasible.

## Reference

- `references/professional-media-workflow.md` — Whole → Detail → Whole
- `references/media-capability-performance.md` — graceful capability ladder
- `references/hero-cinematic-storyboarding.md` — Hero Choreography Brief