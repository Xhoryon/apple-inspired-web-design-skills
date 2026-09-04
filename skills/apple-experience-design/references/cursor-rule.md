# Cursor Non-Replacement Rule (Web)

**Date:** 2026-09-02
**Status:** INTERNAL — Foundation Batch 1

## Rule

> On Web, the native cursor remains user-controlled. The Skill implements highlight surfaces that retarget around the cursor. The Skill does NOT replace the native cursor.

## What this prohibits

- Replacing the native cursor with a custom-drawn one.
- Adding cursor magnetism (cursor snaps to target).
- Adding cursor lag (slowing the cursor to give the highlight time to catch up).
- Adding fake cursor trajectory (parabolic or eased cursor motion).
- Hiding the native cursor while showing a custom replacement.

## What this permits

- A separate highlight surface (e.g. a `::before` element or a dedicated DOM node) that animates between targets.
- The native cursor moves under user control. The highlight surface moves under the implementation's control.
- The highlight may "snap" to a new target — the cursor does not.

## Why this rule exists

The native cursor is the user's primary control signal. Hijacking it (replacement, magnetism, lag, fake trajectory) reduces the user's sense of control. Apple HIG on pointing devices and the platform-applicability matrix agree: pointer effects should communicate state, not control the cursor.

## What the implementation looks like

A common pattern:

- The control is a `<button>` (or `<a>`, or a custom element with `role="button"`).
- A separate `::before` (or sibling) element is the highlight surface.
- The highlight's `transform` animates between target positions.
- The native cursor moves with the user's input device, unchanged.

A `cursor: pointer` style on the control is appropriate (it tells the user the control is clickable). It does NOT move the cursor.

## Where this rule applies

- **Web** (any browser, any input modality) — this rule applies on Web.
- **Native iOS / iPadOS / macOS** — the system cursor / pointer is owned by the system. Custom Web views that embed in a native shell follow the Web rule.
- **Touch Web** — the cursor concept does not apply (no pointer device). The rule does not conflict; it simply does not apply.

## What this rule does NOT do

It does NOT prohibit visual effects that make the highlight more prominent (glow, scale, color change). It prohibits those effects from being applied to the cursor itself.

It does NOT prohibit cursor styling via CSS (e.g. `cursor: pointer` to indicate clickability). It prohibits cursor replacement and cursor magnetism.

It does NOT prohibit JavaScript that animates the page in response to pointer events. It prohibits JavaScript that hijacks the cursor position.

## Cross-Skill note

This rule is enforced in:

- `apple-experience-design/SKILL.md` — primary reference
- `apple-pointer-interaction/SKILL.md` — pointer-target relationship
- `apple-motion-physics/SKILL.md` — motion physics

All three Skills cite this rule. The matrix in `APPLE-EXPERIENCE-PLATFORM-MATRIX.md` records the verdict (`ABANDON` on Web for pointer magnetism) consistent with this rule.
