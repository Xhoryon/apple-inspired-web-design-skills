# Scroll Cinematic Storytelling

## Core correction: scroll is progress, not direction

Hard Visual Media principle: **scroll direction is an input / progress signal, not a required visual-motion direction.**

A downward vertical scroll may legitimately drive:

- horizontal translation
- scale
- framing / depth
- object rotation
- opacity
- mask / reveal
- media-frame progression
- text hierarchy changes
- layout recomposition
- background changes
- annotations
- transition into another section

Do NOT assume:

```
vertical scroll ↓
= visual content must move vertically ↓
```

## Input Axis vs Presentation Axis

Layer C distinction:

- **Input Axis** — what the user physically does (e.g. vertical scroll, pointer drag, keyboard, voice).
- **Presentation Axis** — what the user visually perceives (e.g. horizontal product progression, scale, depth, typography recomposition).

The two may differ. Example: vertical scroll input → horizontal product progression presentation. Or: vertical scroll → scale + horizontal translation + text recomposition. Or: vertical scroll → camera-like depth progression.

Do NOT invent Apple terminology for this distinction.

## Narrative Progress abstraction

Complex cinematic scenes should reason conceptually as:

```
Physical Input
      ↓
Narrative Progress
      ↓
Scene State
      ↓
Presentation Channels
```

Possible presentation channels (not exhaustive; not every scene uses every channel):

- X position
- Y position
- scale
- rotation
- framing
- crop
- mask
- opacity
- media frame
- video time
- typography
- annotation
- background
- layout state

A scene is a composition of media + typography + background + layout + annotations + product/object + framing + controls where appropriate. Cinematic composition is NOT limited to image / video / 3D movement.

## Core model

```
Scroll / narrative progress
      ↓
Narrative state
      ├── media state
      ├── camera / framing state
      ├── text / annotation state
      └── scene state (typography, layout, background, controls)
```

Do not model complex cinematics as unrelated:

```javascript
if (scrollY > X) {
  play animation;
}
```

unless implementation genuinely requires an event boundary.

## Cross-Axis Storytelling

Meaning: the user progresses through one input axis while the visual narrative unfolds primarily along another presentation axis.

Example: vertical scroll input → horizontal visual progression (product family; color / material progression; exterior → interior → component → detail; multiple product states).

Conceptual structure:

```
normal vertical page
↓
bounded cinematic section
↓
A → B → C → D horizontally
↓
normal vertical page continues
```

This is a valid presentation strategy. It is NOT automatically recommended. Do not use it for every section.

## Cross-Axis Storytelling ≠ Scroll Hijacking

Hard invariant: **mapping vertical progress to horizontal / cinematic presentation must not unnecessarily hijack normal scrolling or user agency.**

The user should retain, where the interaction model supports it:

- understandable page progress
- ability to reverse
- ability to continue beyond the scene
- predictable input
- orientation within the experience
- reasonable escape from long cinematic regions

Reject:

- endless-feeling pinned sections
- wheel / trackpad interception that breaks expected page navigation
- trapping the user inside a cinematic
- arbitrary custom inertia replacing normal scroll
- direction changes that disorient without narrative justification

Do NOT turn this into a universal ban on sticky / pinned scenes. Bounded pinned storytelling may be appropriate.

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

## Touch / narrow adaptation

Coordinate with `apple-adaptive-structure`. Desktop may use vertical input → horizontal cinematic presentation; narrow / touch contexts may adapt to a normal vertical editorial progression (A ↓ B ↓ C ↓ D).

Core principle: **preserve narrative intent, not literal cross-axis mechanics.**

On Touch Web, a horizontally moving scene driven by vertical scroll may visually suggest horizontal swiping. Consider whether:

- interaction remains understandable
- a swipe affordance is falsely implied
- touch users may try direct horizontal manipulation
- a vertical editorial fallback is clearer

Do NOT make cross-axis storytelling mandatory on touch.

## Typography / Layout recomposition

A scene may transform typography through:

- large statement → shrink gradually → reposition into normal section hierarchy → reveal product / media / content

This is **Narrative Recomposition**, not merely "headline fades out." The Skill reasons about the semantic transition (Hero-level statement → normal content-level heading) where appropriate.

Layout may participate in scene choreography:

centered single-statement composition
→ text + product
→ product + annotation
→ normal editorial section

Ownership boundary:

- `apple-visual-media-composition` owns narrative intent of scene recomposition and the media / text / layout visual relationship through the cinematic sequence.
- `apple-adaptive-structure` owns structural adaptation due to available space / platform / context.
- `apple-motion-physics` owns motion mechanics / continuity / easing / interruptibility.

## Media-light Scene Choreography

Important principle: **cinematic composition does not require video or 3D.** A high-quality scene may use typography, static images, masks, scale, layout recomposition, horizontal translation, subtle parallax, and background transformation.

Do NOT use advanced media merely because cinematic storytelling is desired.

## Reference

- `references/hero-cinematic-storyboarding.md` — Hero choreography + Visual Beats (Hero is one important use case; Scene Choreography is the broader concept)
- `references/media-accessibility.md` — reduced motion + accessibility
- `references/visual-inspection-evidence.md` — video / asset evidence provenance
- `references/professional-media-workflow.md` — Whole → Detail → Whole
- `APPLE-EXPERIENCE-CAPABILITY-EVIDENCE-CONTRACT.md` — video semantic conservatism, Opaque Media, generation ≠ inspection
- `APPLE-EXPERIENCE-ACCESSIBILITY-CONTRACT.md` — cross-cutting accessibility invariants