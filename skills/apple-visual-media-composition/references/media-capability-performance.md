# Media Capability & Performance

## Reason about

- asset size
- memory
- decode cost
- GPU
- network
- battery where relevant
- frame pacing
- mobile capability
- browser support
- 3D support
- video seeking
- image sequence preload

## Graceful capability ladder

A cinematic may adapt:

```
Real-time 3D
↓
pre-rendered sequence / video
↓
composed 2D motion
↓
static key visual
```

Only when these preserve the intended meaning sufficiently. **Reliable simpler media is better than broken advanced media.**

## Network / device / GPU capability

- Mobile: lower decode budget; smaller asset; shorter sequence.
- Desktop: larger asset; longer sequence; potentially 3D.
- Watch: limited; small images; minimal motion.
- TV: focus-driven; pre-rendered cinematic.
- VR: spatial; gaze + pinch; 3D.

Detect capability before promising 3D / video / image-sequence work.

## Asset reality vs implementation budget

Implementation budgets are project heuristics unless sourced. Do not invent:

> "Apple-quality Hero must stay under N MB."

Use contextual judgment.

## Reference

- `references/asset-feasibility-authenticity.md` — V0..V5 ladder; P0..P4 ambition
- `references/hero-cinematic-storyboarding.md` — Hero complexity
- `references/scroll-cinematic-storytelling.md` — scroll performance