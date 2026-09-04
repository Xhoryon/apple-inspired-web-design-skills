# Fidelity Audit Reference

The Liquid Glass Fidelity Profile — a multidimensional evaluation framework. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

The Profile is independent of the **Implementation Capability Level** (the stable v1.1 Level 0–3 ladder). A Level 1 implementation can achieve high Material, Adaptivity, Interaction, and Accessibility fidelity even though it has low Optical fidelity. The two are independent.

## The eight dimensions

The Profile has eight dimensions. Each is scored on its own scale; the profile is the combined picture, not a single linear number.

1. **Material Fidelity** — see `material-fidelity.md`.
2. **Optical Fidelity** — see `optical-fidelity.md`.
3. **Adaptivity Fidelity** — see `SKILL.md` (Adaptivity section) and `material-fidelity.md`.
4. **Interaction Fidelity** — see `glass-interaction.md`.
5. **Morphing Fidelity** — see `glass-morphing.md`.
6. **Hierarchy / Semantic Fidelity** — does the glass surface express its role in the interaction hierarchy?
7. **Accessibility Fidelity** — see `glass-legibility.md` and `web-capability-fallback.md`.
8. **Performance Fidelity** — see `web-capability-fallback.md` and the stable v1.1 baseline.

The Profile does NOT include "more optical effects = higher fidelity". A glass surface with restraint and good Adaptivity can score higher than a theatrical Level 3 surface with bad Adaptivity and bad Accessibility.

## The scoring rubric (per dimension)

Each dimension is scored 0–3:

- **0** — dimension is absent or wrong.
- **1** — dimension is present but minimal.
- **2** — dimension is present and correct.
- **3** — dimension is exemplary.

The implementer scores each dimension independently. The total is a single number (0–24), but the profile is the dimensional map. A surface that scores 24 is a top-tier example; a surface that scores 6 may be appropriate if the design intent is minimal.

### Material Fidelity

- **0** — the surface is a constant fill with no translucency, no edge, no adaptivity.
- **1** — the surface is a translucent fill with a basic border; not adaptive.
- **2** — the surface is adaptive (light / dark / image), has a visible edge, and uses the right variant (regular-like or clear-like) for its context.
- **3** — the surface is exemplary: subtle luminosity adaptation, semantic tint, clear hierarchy, legibility on every tested backdrop.

### Optical Fidelity

- **0** — the surface has no optical response; it is a flat pill.
- **1** — the surface has a static gradient highlight (decoration, not response).
- **2** — the surface has pointer-tracked specular response (rAF-throttled, idle-paused) and adaptive translucency.
- **3** — the surface has restrained refraction (Level 2 or Level 3) and the optical effect is in service of the content, not the visual subject.

A surface that scores 3 on Optical Fidelity but 0 on Material Fidelity is a theatrical effect; the implementer is asked to reconsider.

### Adaptivity Fidelity

- **0** — the surface is constant across all backgrounds and user preferences.
- **1** — the surface adapts to light / dark mode only.
- **2** — the surface adapts to light / dark / image / busy backgrounds and respects Reduce Transparency and Increase Contrast.
- **3** — the surface adapts to all of the above plus input modality (touch vs. pointer vs. keyboard) and is tested on the actual target browsers.

### Interaction Fidelity

- **0** — the surface has no state response; it looks the same in idle, hover, pressed, focus, selected.
- **1** — the surface has at most one state response (e.g. hover only).
- **2** — the surface has state responses for all documented states, and the responses are perceptible but restrained.
- **3** — the surface has exemplary state responses, including focus visibility, selected persistence, and disabled state distinctness.

### Morphing Fidelity

- **0** — the surface does not morph; it snaps between states.
- **1** — the surface morphs but the morph is not semantically meaningful.
- **2** — the surface morphs with semantic meaning (e.g. segmented control selection), and the morph uses the retargeting rule.
- **3** — the surface morphs with semantic meaning, retargets mid-flight, and integrates with the surrounding hierarchy (e.g. sheet source continuity).

### Hierarchy / Semantic Fidelity

- **0** — the surface does not express its role (e.g. a "primary action" button that looks identical to a "secondary" button).
- **1** — the surface has a visual hierarchy but the hierarchy does not match the semantic role.
- **2** — the surface expresses its role (e.g. primary action is more prominent; secondary is less).
- **3** — the surface expresses its role, integrates with the surrounding hierarchy, and the role is consistent across the page.

### Accessibility Fidelity

- **0** — the surface fails WCAG contrast on the worst-case backdrop, or does not respect Reduce Transparency / Increase Contrast / Reduce Motion.
- **1** — the surface passes WCAG contrast on the tested backdrops but does not respect one or more accessibility preferences.
- **2** — the surface passes WCAG contrast and respects all three accessibility preferences.
- **3** — the surface passes WCAG contrast, respects all three preferences, and the focus indicator is visible on every backdrop.

### Performance Fidelity

- **0** — the surface is jank, lag, or causes a frame drop.
- **1** — the surface is acceptable on desktop but janky on mobile.
- **2** — the surface is acceptable on desktop and mobile.
- **3** — the surface is acceptable on desktop, mobile, and low-end devices; the Governor down-shifts gracefully.

## Anti-pattern candidates (Batch 3 internal)

The Skill adds the following candidates to the internal anti-pattern list:

- **Shader Supremacy** — WebGL / shader on every surface regardless of need.
- **Glass Everywhere** — every card, every panel, every section is glass.
- **Optical Cosplay** — claiming chromatic aberration / DOF / anisotropy as "Apple behavior" without evidence.
- **Clear-on-Complex** — using a Clear-like variant where the user needs to read.
- **Broken Glass Group** — multiple nearby glass surfaces that produce obviously inconsistent sampling.
- **Morph Without Meaning** — morphing unrelated controls or decorative-only morphs.

These are recorded for future v1.x inclusion in the public anti-pattern library. They are NOT added to the public v1.1.0 library in this batch.

## The audit pattern

The implementer audits a surface by:

1. **Scoring each dimension** 0–3.
2. **Comparing the score to the design intent.** A surface that scores 3 on Optical but 0 on Accessibility may not be the right surface; the implementer is asked to reconsider.
3. **Cross-checking the source citations.** Every Apple-specific claim must be in the source matrix with an evidence class.
4. **Cross-checking the anti-pattern list.** The surface must not match any rejected anti-pattern.
5. **Cross-checking the Level ladder.** The implementation capability should match the design intent, not the other way around.

The audit is a separate step from the implementation. The implementer may run the audit before shipping; the team may run the audit as part of code review.

## What the Skill rejects

- A single linear "fidelity score" that counts optical effects.
- A glass surface that is theatrical (high Optical, low Accessibility).
- A glass surface that is a flat pill (low Material, low Optical).
- A glass surface that ignores user preferences (Reduce Transparency, Increase Contrast, Reduce Motion).
- A glass surface that morphs without semantic meaning.
- A glass surface that is illegible on a busy backdrop.

## Cross-Skill note

- The 7-dimension model (translucency, refraction, specular, adaptivity, depth, dynamics, morphing) is the stable v1.1 baseline. The Profile is the deepening of those dimensions plus four more (Interaction, Morphing, Hierarchy, Accessibility, Performance).
- The Level 0–3 ladder is the **Implementation Capability Level**, separate from the Profile.
- The accessibility settings (Reduce Transparency, Increase Contrast, Reduce Motion) are inherited from `apple-control-states` and the stable v1.1 baseline.
- The source matrix is `APPLE-EXPERIENCE-LIQUID-GLASS-SOURCE-MATRIX.md`.
- The candidate anti-patterns are in `apple-experience-foundation/candidate-anti-patterns.md` (to be updated in the Batch 3 development log).
