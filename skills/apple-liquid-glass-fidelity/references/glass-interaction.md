# Glass Interaction Reference

The Material state model — how the glass material responds to the semantic state of the control it belongs to. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

> **HARD RULE:** The semantic state (idle, hover, pressed, focused, selected, disabled) is owned by `apple-control-states`. This Skill describes how the **material** responds to the state, not what the state is.

The state table follows the same order as `apple-control-states`. Each row states the **perceptual intent** of the material response. Specific motion values (spring stiffness, amplitude, duration) remain HEURISTIC owned by `apple-motion-physics`.

## The state table

### `idle`

**Material intent:** the surface is at rest. No specular movement, no refraction animation, no morph in progress.

- Translucency: at its baseline value for the surface.
- Specular highlight: positioned at a default location (often the top edge).
- Refraction: stable.
- Morph: not in progress.
- Motion: none.

### `hover`

**Material intent:** the surface acknowledges the pointer with a local, restrained specular highlight. The user knows the surface is interactive.

- Translucency: unchanged from `idle` (or very slightly increased).
- Specular highlight: a small, soft highlight follows the pointer. The highlight is rAF-throttled and idle-pauses after 200 ms of no pointer movement.
- Refraction: unchanged (no animation on hover).
- Morph: not in progress.
- Motion: low-amplitude spring; brief transition (e.g. 120–180 ms).
- Gating: **(hover: hover) and (pointer: fine)**. Touch-only Web does not get hover; touch uses `pressed` + `selected` instead.
- See also: `apple-pointer-interaction/references/pointer-modality.md` and `cursor-rule.md`.

### `pressed`

**Material intent:** the surface gives tactile, fast, recoverable feedback. The user knows their input is being received.

- Translucency: slightly increased (the surface "compresses" the backdrop).
- Specular highlight: brighter, more local, follows the pointer precisely.
- Refraction: unchanged (no animation on press).
- Morph: not in progress.
- Motion: input-sensitive amplitude (touch > mouse > trackpad per `apple-motion-physics/references/input-sensitive-amplitude.md`); fast recovery (200–300 ms).
- The exact amplitude is a HEURISTIC, not Apple canonical. The Skill recommends a perceptible but restrained press; "scale 0.96" is unsupported and would mislead.

### `focused`

**Material intent:** the surface gains a visible boundary or highlight that does **not** depend on pointer state. The user (especially keyboard users) sees the focus.

- Translucency: unchanged.
- Specular highlight: a small static highlight at the focus edge.
- Refraction: unchanged.
- Morph: not in progress.
- Motion: discrete or very brief transition (≤ 80 ms).
- Focus indicator: a 2 px outline outside the surface; visible against any backdrop. The focus indicator must NOT be blurred into invisibility.
- See also: `apple-control-states/references/focus-vs-hover-vs-selection.md`.

### `selected`

**Material intent:** the surface becomes the active identity. It may be the only selected surface in a group; it may persist after pointer exit.

- Translucency: slightly more pronounced (the surface "lifts").
- Specular highlight: a stable highlight at the selected position; not pointer-tracked.
- Refraction: unchanged.
- Morph: the surface may enter or exit a morphing group (e.g. segmented control selection morph, see `glass-morphing.md`).
- Motion: spring (retarget from current state per `apple-motion-physics/references/retargeting.md`); brief transition.
- The selected state **persists** after pointer exit. Removing the selected state requires an explicit user action.

### `disabled`

**Material intent:** the surface is present but not interactive. It should be visibly less prominent.

- Translucency: slightly reduced (the surface "fades" toward the backdrop).
- Specular highlight: none.
- Refraction: unchanged.
- Morph: not in progress.
- Motion: discrete or none.
- The disabled state **must not** show hover or press treatment. A disabled glass surface with a hover highlight is a defect.
- The disabled state **must** remain accessible (correct `aria-disabled` / `disabled`).
- See also: `apple-control-states/references/disabled-state.md`.

### `loading`

**Material intent:** the surface shows a non-distracting progress indicator. The user knows an action is in flight.

- Translucency: unchanged.
- Specular highlight: may rotate slowly (subtle, not animated on every frame).
- Refraction: unchanged.
- Morph: not in progress.
- Motion: looped, restrained; respects `prefers-reduced-motion` (instant state change instead of loop).
- The loading state must NOT be a full spinner inside a glass surface. The skill recommends a subtle progress bar or a micro-indicator.
- See also: `apple-control-states/references/loading-state.md`.

## The cursor rule (Web)

The native cursor is the user's primary control signal. The glass material does **not** replace the cursor and does **not** add cursor magnetism. The highlight surface (if any) follows the pointer near the target; the cursor remains user-controlled.

See: `apple-pointer-interaction/references/cursor-rule.md`.

## Input-sensitive amplitude

The press feedback amplitude depends on the input modality:

- **Touch:** larger amplitude (the touch surface is the entire surface). See `apple-motion-physics/references/input-sensitive-amplitude.md`.
- **Mouse / trackpad:** smaller amplitude (precise pointer).
- **Keyboard:** no press; Enter / Space activates.

The values are HEURISTIC. The implementer tunes per the design.

## Shared highlight glass and selection

A shared highlight glass (segmented control selection) follows the **retargeting rule** from `apple-motion-physics/references/retargeting.md`. The highlight retargets from its current state when the selection changes; it does NOT "finish A then start B".

The shared highlight is a **single material surface**. It is not multiple cross-fading surfaces. See `glass-groups.md` for the implementation pattern.

### Selection-state techniques within one material family (canonical)

Within one material family (one Regular-like cluster, or one Clear-like cluster — never a mix), selection state is expressed through:

- **Tint** — a deliberate semantic overlay color applied to the selected item within the same variant.
- **Foreground treatment** — text / icon color and weight change.
- **Luminance treatment** — slightly brighter or darker fill on the selected item within the same variant.
- **Border / highlight** — a 1 px hairline border or an inset highlight on the selected item.
- **Shared geometry** — a single shared highlight surface (one element, not multiple) that retargets between sibling targets within the same variant.
- **Semantic selection state** — `aria-selected="true"` / `[data-selected]`; the state is persistent, distinct from hover and focus.

The Skill does NOT introduce a Clear-like patch to express selection inside a Regular-like cluster (or vice versa). That would mix variant families within one group, violating the canonical no-mixing invariant. See `glass-groups.md` §"No-mixing invariant (canonical)" and `material-fidelity.md` §"Do not mix Regular-like and Clear-like within one design treatment".

## Motion ownership

All motion values (spring stiffness, amplitude, duration, easing) are owned by `apple-motion-physics`. The Skill describes perceptual intent; the implementer chooses the values.

## What the Skill rejects

- A glass surface that is always animated (no `idle` state).
- A glass surface with a hover effect on touch-only Web.
- A glass surface that loses its selected state on pointer exit.
- A glass surface that shows press treatment when disabled.
- A glass surface with a focus indicator that is blurred into invisibility.
- A glass surface that animates on every frame (no idle-pause).
- A glass surface whose highlight is decoration (static) rather than response (pointer-tracked).
- Mixing Regular-like and Clear-like within one control cluster / sheet / popover / design treatment to express selection. Selection is expressed via tint / foreground / luminance / border / shared highlight / semantic state within the one family.

## Cross-Skill note

- The semantic state catalog is `apple-control-states`. This Skill cites, not duplicates.
- The motion values (spring, amplitude, duration) are `apple-motion-physics`.
- The pointer-target relationship (hover, cursor, retarget) is `apple-pointer-interaction`.
- The retargeting rule is `apple-motion-physics/references/retargeting.md`.
- The reduced-motion rule is `apple-motion-physics/references/reduced-motion.md`.
- The legibility model (text on glass) is `glass-legibility.md`.
- The no-mixing invariant (one variant per group) is enforced at the group level in `glass-groups.md`.
- The selection-state techniques within one material family are described here and in `material-fidelity.md`.
