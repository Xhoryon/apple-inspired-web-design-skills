---
name: apple-experience-design-routing-examples
description: Extended routing examples for the apple-experience-design orchestrator. Tier 2 reference moved out of SKILL.md to keep Tier 1 within the 1500-word budget while preserving all v0.2 / v0.3 / v0.4 routing behavior.
version: "0.4.0-dev"
license: MIT
---

# Routing examples (Tier 2)

## Example 1 — "Make this Web button feel more like iOS."

- Platform: Desktop Web. Input: mouse.
- Behavior: press feedback.
- Verdict: DIRECT.
- Loads: `apple-control-states` (Required), `apple-motion-physics` (Conditional).
- Usually skip: `apple-pointer-interaction`, `apple-navigation-spatial`, `apple-direct-manipulation`, `apple-modality-overlays`, `apple-adaptive-structure`, full v1.1 audit.

## Example 2 — "Make this iPhone-like mobile Web control react like iOS."

- Platform: Touch Web / PWA. Input: touch only.
- Behavior: press feedback.
- Verdict: DIRECT.
- Loads: `apple-control-states` (Required), `apple-motion-physics` (Conditional).
- Does NOT load `apple-pointer-interaction` (ABANDON on touch-only).
- Does NOT load `apple-navigation-spatial`, `apple-direct-manipulation`, `apple-modality-overlays`, `apple-adaptive-structure`.

## Example 3 — "Build a native iOS button from scratch."

- Platform: Native iOS. Input: touch.
- Behavior: press.
- Verdict: NATIVE.
- Recommendation: use `UIButton`. Custom re-implementation requires a justification.
- Loads: stable v1.1 reference for component grammar. Does NOT load experimental Skills.

## Example 4 — "Add iPadOS pointer highlight to a desktop Web component."

- Platform: Desktop Web. Input: mouse.
- Behavior: pointer highlight retargeting (iPadOS 26-style).
- Verdict: APPROXIMATE.
- Loads: `apple-control-states` (Required), `apple-pointer-interaction` (Required), `apple-motion-physics` (Required).
- Tag as APPROXIMATE. Enforce Web cursor rule.

## Example 5 — "Add iPadOS pointer highlight to a touch-only phone website."

- Platform: Touch Web. Input: touch only.
- Behavior: pointer highlight.
- Verdict: ABANDON.
- Reject. Replace with press + selection + touch feedback. `apple-pointer-interaction` not loaded.

## Example 6 — "Click list item → detail page, with back that restores scroll position."

- Platform: Desktop Web. Input: mouse.
- Behavior: list → detail navigation, back, scroll restoration.
- Verdict: DIRECT.
- Loads: `apple-navigation-spatial` (Required), `apple-motion-physics` (Conditional), stable v1.1 `apple-web-composition` (Conditional), stable v1.1 `apple-ui-components` (Conditional).
- If the task also requires structural reorganization across sizes / windows / inputs: load `apple-adaptive-structure` (Conditional).

## Example 7 — "User can drag a card to reorder a list on iPad."

- Platform: Native iPadOS. Input: touch.
- Behavior: drag-to-reorder.
- Verdict: NATIVE. Use `UIKit` drag interaction / SwiftUI `.draggable` / `.onMove`.

## Example 8 — "User can drag a card on Web with smooth follow."

- Platform: Desktop Web. Input: mouse.
- Behavior: drag-to-reorder.
- Verdict: DIRECT.
- Loads: `apple-direct-manipulation` (Required), `apple-control-states` (Required), `apple-motion-physics` (Required).

## Example 9 — "Button opens a sheet for editing."

- Platform: Native iOS. Input: touch.
- Behavior: sheet.
- Verdict: NATIVE. Use `UIPresentationController` / SwiftUI `.sheet`.

## Example 10 — "Button opens a sheet for editing on Web."

- Platform: Desktop Web. Input: mouse.
- Verdict: DIRECT.
- Loads: `apple-modality-overlays` (Required), `apple-control-states` (Required), `apple-motion-physics` (Conditional), `apple-direct-manipulation` (Conditional if interactive dismiss).
- Does NOT load `apple-adaptive-structure` unless the task also requires structural reorganization.

## Example 11 — "Recreate iOS edge swipe back on Web."

- Verdict: ABANDON. Do NOT implement custom edge swipe back on Web. Browser's native Back is correct.

## Example 12 — "Right-click menu with actions on a list."

- Verdict: DIRECT.
- Loads: `apple-modality-overlays` (Required), `apple-control-states` (Required), `apple-pointer-interaction` (Conditional).

## Example 13 — "Same app should work on iPhone, iPad, and Mac with adaptive structure."

- Platform: Multi-platform. Behavior: structural reorganization across platforms.
- Verdict: NATIVE on Apple, DIRECT on Web.
- Loads: `apple-adaptive-structure` (Required). Conditional: `apple-navigation-spatial` (if anatomy includes nav), `apple-modality-overlays` (if auxiliary surfaces are modal), stable v1.1 `apple-web-composition` (Web composition), stable v1.1 `apple-ui-components` (component grammar).

## Example 14 — "At narrow width the sidebar collapses to a tab bar."

- Loads: `apple-adaptive-structure` (Required), `apple-navigation-spatial` (Required if the rail is a navigation surface).

## Example 15 — "Animate the split collapse."

- Loads: `apple-adaptive-structure` (Required), `apple-motion-physics` (Required for the animation).

## Example 16 — "Make the Web page look like macOS."

- Verdict: ABANDON. Visual imitation does NOT trigger structural adaptation. Visual design lives in stable v1.1 `apple-design-foundations` + `apple-web-composition`. The Skill rejects "Web = macOS visual imitation" as not adaptive structure.

## Example 17 — "Ordinary responsive marketing page on iPhone / iPad / desktop."

- Verdict: DIRECT on Web.
- Loads: stable v1.1 `apple-web-composition` (Required), stable v1.1 `apple-design-foundations` (Required).
- Does NOT load `apple-adaptive-structure` — the request is page composition, not structural reorganization. (Marketing pages reflow with viewport / container queries; that is composition, not anatomy preservation.)

## Example 18 — "Three-column collapse when window shrinks on iPad."

- Loads: `apple-adaptive-structure` (Required), `apple-navigation-spatial` (Required).

## Example 19 — "Stage Manager window resize changes the multi-column."

- Loads: `apple-adaptive-structure` (Required).

## Example 20 — "Static Liquid Glass request — apply glass to a card."

- Verdict: visual. Loads stable v1.1 `apple-liquid-glass-web` (baseline). If adaptive / interactive / morphing behavior is required, add experimental `apple-liquid-glass-fidelity`. Does NOT load `apple-adaptive-structure` unless structural reorganization is also required.
