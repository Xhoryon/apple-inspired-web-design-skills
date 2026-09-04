# Scroll Cinematic Storytelling

## Core model

```
Scroll / narrative progress
      ↓
Narrative state
      ├── media state
      ├── camera / framing state
      └── text / annotation state
```

Do not model complex cinematics as unrelated:

```javascript
if (scrollY > X) {
  play animation;
}
```

unless implementation genuinely requires an event boundary.

## Reversibility

Major hard invariant: **Scrubbed scroll-controlled visual states should reverse coherently when scroll reverses.**

If downward scrolling produces:

```
Establish → Rotate → Detail → Transition
```

upward scrolling should restore coherent earlier visual states. Do not leave stale media / camera / text state behind.

## Reversible does not mean literal film reversal

Do not reverse user data or user choices. Differentiate:

- **Scroll-owned state** — often reversible.
- **User-owned state** — normally persistent.

Do not reset selected product color just because the user scrolls upward.

## Cinematic explanations

Hero / cinematic sections may include:
- feature annotations
- contextual explanation
- text that enters / exits based on narrative state
- detail explanation

But text must remain:
- readable
- sufficiently persistent
- accessible
- meaningful without relying only on motion

Do not create fleeting unreadable captions purely for spectacle.

## Reduced motion preserves narrative

Major gate. **Reduced motion must preserve the narrative, not merely delete the animation.**

Possible fallback:

```
Full: rotate → approach → detail transition
Reduced: key pose A → fade → key pose B → detail
```

Preserve:
- feature ordering
- explanation
- destination
- information hierarchy

Do NOT simply set `animation-duration: 0` if doing so destroys the visual story.

## Reference

- `references/hero-cinematic-storyboarding.md` — Hero choreography + Visual Beats
- `references/media-accessibility.md` — reduced motion + accessibility
- `references/professional-media-workflow.md` — Whole → Detail → Whole