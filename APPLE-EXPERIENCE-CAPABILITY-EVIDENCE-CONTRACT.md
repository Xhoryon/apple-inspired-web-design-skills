# Apple Experience — Capability & Evidence Contract

**Date:** 2026-09-03
**Status:** Public cross-cutting contract
**Scope:** project-level cross-cutting contract applicable across all Skills and tests
**Owner:** project (not Skill 14; not a per-Skill concern)

This contract governs what this Agent can actually observe, execute, generate, inspect, and prove. It is NOT a generic AI governance document. It is narrowly focused on evidence provenance for Apple Experience Skills.

---

## Purpose

Establish a project-level rule so that:

1. The current Agent never claims a capability it does not possess.
2. The current Agent never claims perceptual understanding it does not possess.
3. Evidence provenance survives delegation.
4. Generation ≠ inspection.
5. Inspection ≠ generation.
6. Each Skill and test verdict carries an honest evidence label.

---

## Capability Dimensions

Capability is task-specific. Do NOT model as binary CAN / CANNOT globally.

An Agent may be able to:
- read image metadata (dimensions / format / EXIF / alpha)
- write CSS / run browser tests
- inspect file structures

while being unable to:
- semantically inspect image pixels (subject / focal point / composition)
- generate video
- run native SwiftUI / UIKit / AppKit runtime
- inspect GLB visuals

---

## Evidence Provenance

Introduce explicit provenance labels that integrate with the existing evidence-type taxonomy:

| Label | Meaning |
|---|---|
| `SOURCE EVIDENCE` | Primary documentary evidence (Apple DocC / HIG / SDK / W3C). |
| `STATIC CANONICAL` | The Skill's own text agrees with the rule. |
| `PROTOTYPE CODE` | Implementation exists in prototype HTML / code. |
| `BROWSER RUNTIME` | Actual browser behavior executed + observed (Chromium / Safari). |
| `NATIVE RUNTIME` | Actual native runtime executed + observed. |
| `ASSET INSPECTION` | File-level inspection (dimensions / format / EXIF / codec / frame count). Does NOT include semantic understanding. |
| `ROUTER TEST` | Orchestrator routing behavior verified. |
| `ADVERSARIAL TEST` | Counter-prompt rejected per canonical guidance. |
| `MIXED` | Multiple evidence types apply. |
| `DIRECT VISUAL INSPECTION` | Current Agent actually inspected media semantically (pixels). |
| `DELEGATED VISUAL INSPECTION` | Another capable Agent inspected and produced a traceable structured report. |
| `USER-SUPPLIED DESCRIPTION` | A professional / user supplied semantic information. |

---

## Technical Asset Inspection vs Semantic / Visual Inspection

These are distinct categories. Never collapse them.

### Technical Asset Inspection

May include:
- file format
- dimensions / aspect ratio
- alpha / transparency
- file size
- metadata
- video duration / codec / frame count
- model file type
- asset structure

May often be performed without visual understanding.

### Semantic / Visual Inspection

May include:
- what is depicted
- subject identity / position
- focal point
- safe crop
- composition / hierarchy
- image consistency
- product geometry
- visual defects
- frame-to-frame continuity

Requires actual perceptual evidence.

Reading metadata does NOT equal understanding media content.

---

## Direct vs Delegated Verification

### Direct Visual Inspection

Current Agent actually inspected the media semantically. The Agent may assert visual claims.

### Delegated Visual Inspection

Another capable Agent inspected and produced a traceable structured report. The current Agent may cite the report but may NOT upgrade the label to `DIRECT VISUAL INSPECTION`.

**Hard invariant:** Evidence provenance survives delegation.

A Coding Agent that reads `VISUAL-ASSET-AUDIT.md` written by a Vision Agent may say:

> Based on delegated visual inspection, the subject is center-right.

It may NOT say:

> I visually confirmed the subject is center-right.

---

## Generation ≠ Inspection

**Major invariant:** Generation capability does NOT imply verification capability.

| Agent capability | Implication |
|---|---|
| Can generate an image | CANNOT self-certify identity consistency / geometry / hallucinated components |
| Can generate video | CANNOT self-certify temporal continuity / correct frame count |
| Can generate 3D asset | CANNOT self-certify runtime render quality |
| Can generate multi-view product sequence | CANNOT self-certify animation-ready continuity |

If generated content requires visual / factual QA and the current Agent cannot perform it, route to:
- a capable visual Agent
- user confirmation
- another valid inspection mechanism
- or a lower-risk fallback

Do not self-certify unseen output.

---

## Inspection ≠ Generation

Likewise:
- An Agent capable of understanding an image does NOT automatically have image-generation capability.
- An Agent capable of video inspection does NOT automatically have video-generation capability.

Keep these dimensions separate.

---

## Runtime Boundaries

The same evidence-provenance principle applies to runtime claims.

| Claim | Evidence required |
|---|---|
| `BROWSER RUNTIME` | Actual browser behavior executed + observed (Chromium / Safari). |
| `NATIVE RUNTIME` | Actual native runtime executed + observed. |
| `3D RUNTIME` | Actual 3D scene rendered + observed. |
| `VIDEO RUNTIME` | Actual video seek / scrub / cinematic quality observed. |
| `AUDIO RUNTIME` | Perceptual audio behavior observed. |
| `PROTOTYPE CODE` | Implementation exists; runtime may not have been executed. |

Code exists ≠ runtime verified. A React prototype compiles ≠ Safari runtime verified.

---

## Required Escalation / Fallback

Where a decision materially depends on unavailable perception or runtime:

1. **Recognize the gap early.** Surface the missing evidence type in the Plan / Task / Note.
2. **Choose a safer pathway.** Demote the claim to PARTIAL Type A; mark unverified; use known safe default.
3. **Escalate appropriately.** Route to a user / professional / capable Agent when visual / runtime verification is required.
4. **Provide a fallback.** Use contained presentation / placeholder / wider safe-crop range / prototype variants.
5. **Never bluff.** A vision-unavailable Agent must not claim vision-derived decisions.

---

## Vision Asset Manifest

For projects that need a structured way to record visual evidence, the following optional schema is recommended. Do NOT make every small image task require a formal manifest. Use Scope-Proportional Process.

```
Asset:
  Role:
  Semantic description:
  Primary subject:
  Focal point:
  Protected region:
  Available negative space:
  Crop tolerance:
  Authenticity:
  Allowed manipulation:
  Technical metadata:
  Visual evidence source:
```

This is `USER-SUPPLIED DESCRIPTION` evidence unless the agent that produced it has been identified as a `DELEGATED VISUAL INSPECTION` source.

---

## Visual Evidence Ladder (Skill / project-owned Layer C taxonomy)

This is Skill-owned vocabulary. Not Apple terminology. Do NOT imply every task must reach VE3.

| Level | Meaning |
|---|---|
| `VE0` | Metadata only. No semantic visual confirmation. |
| `VE1` | User-described. Semantic description supplied by user / client. |
| `VE2` | Delegated visual inspection. A capable vision Agent has inspected and documented the asset. |
| `VE3` | Direct visual + relevant runtime verification. Strong direct visual / runtime evidence where applicable. |

Use the appropriate level; do not pretend a higher level.

---

## Hard Invariants

1. **Never bluff capability.** No image-generation tool → do not promise image generation. No 3D runtime → do not claim realtime 3D verification. No native runtime → do not claim native runtime behavior. No visual perception → do not claim direct image inspection.
2. **Never bluff perceptual understanding.** Do not claim subject position / focal point / visual hierarchy / crop safety / geometry consistency / product correctness / lighting consistency / image quality / animation continuity without legitimate evidence.
3. **Generation ≠ inspection.** Generating an asset does not certify it.
4. **Inspection ≠ generation.** Inspecting an asset does not imply generating it.
5. **Indirect / delegated evidence must remain labeled.** Do NOT upgrade provenance through reading another agent's report.
6. **Metadata ≠ semantic media understanding.** Reading file metadata is technical inspection. It does not establish perceptual understanding. For video specifically, metadata (duration / dimensions / codec / frame count) does not establish shot content / camera movement / visual continuity. Treat video visual content as `UNKNOWN` (Opaque Media mode) unless trustworthy semantic evidence exists.
7. **Three existing evidence layers preserved.** Layer A (Apple official / W3C) / Layer B (Skill synthesis) / Layer C (Skill implementation vocabulary) still apply. This contract sits beside them, not above them.
8. **No new primary Skill.** This is a project-level contract, not a Skill 14.

---

## Examples (operational)

### Example A — Hero implementation without visual Agent

> I have 120 frames of `frame_001.webp` ... `frame_120.webp` and a GLB. I cannot visually inspect them. I will:
> 1. Inspect dimensions / format / frame count (technical).
> 2. Ask the user for an Asset Manifest describing the subject / focal point / protected region / crop tolerance.
> 3. Build the Hero layout with contained framing and reversible scroll narrative state, using the Manifest as `USER-SUPPLIED DESCRIPTION` evidence.
> 4. Surface in the test verdict: `PARTIAL Type A — Hero choreography unverified; relies on USER-SUPPLIED DESCRIPTION + technical inspection`.
> I will NOT claim "smooth 180° rotation verified" without visual evidence.

### Example B — Image generation without visual QA

> The current Agent can generate image assets but cannot semantically inspect them. After generation, the Skill routes visual QA to:
> - a capable visual Agent (DELEGATED VISUAL INSPECTION), OR
> - user confirmation.
> The Agent does NOT self-certify generated output.

### Example C — User description supplied

> The user says the focal point is on the left third. The Skill records this as `USER-SUPPLIED DESCRIPTION`, NOT `DIRECT VISUAL INSPECTION`. The layout may use the description but the verdict must reflect the provenance.

### Example D — Browser prototype vs native runtime

> The prototype compiled and rendered in Chromium. This is `BROWSER RUNTIME`. It does NOT establish `NATIVE RUNTIME`. The Skill labels each evidence separately.

---

## Integration with existing Skills

This contract is referenced by:
- `apple-visual-media-composition` — visual inspection + AI asset QA + Hero choreography.
- `apple-feedback-response` — error / loading states depend on available evidence.
- `apple-motion-physics` — motion continuity / reversibility may require perceptual evidence.
- `apple-adaptive-structure` — responsive art direction may require visual evidence.
- `apple-experience-design` — orchestration may route to visual-capable Agent.

Do NOT mechanically reference this contract from every Skill; reference it where evidence capability materially matters.

---

## Project-level cross-cutting nature

This contract is conceptually:

```
Capability & Evidence Contract
        ↓
    all evidence claims
        ↓
    Source Evidence / Runtime Evidence / Prototype Evidence / Perceptual Evidence / Delegated Evidence
```

It interacts with but does not replace Layer A / B / C evidence discipline. It governs **what this Agent can claim**; it does not govern **what Apple said**.

---

## PARTIAL handling under this contract

Where a decision materially depends on unavailable perception / runtime:

- **Type A PARTIAL**: if canonical logic is complete and the limitation is environmental / evidence-bounded, mark PARTIAL Type A.
- **Type B PARTIAL**: if the canonical process itself fails to request / route necessary evidence (e.g. claims visual verification without invoking Visual Evidence Ladder), mark PARTIAL Type B (blocks verification).
- Do NOT turn missing perceptual evidence into PASS.

---

## Failure modes this contract prevents

- "I read the dimensions, so I know the subject is centered." (REJECT — metadata ≠ semantic understanding.)
- "The filename is hero-right, so the subject is on the right." (REJECT — filename ≠ focal point.)
- "I generated the views, so they are consistent." (REJECT — generation ≠ inspection.)
- "I have 120 frames, so the rotation is smooth." (REJECT — frame count ≠ visual continuity.)
- "GLB parses, so the cinematic is visually verified." (REJECT — parsing ≠ runtime visual behavior.)
- "A Vision Agent said it, so I visually confirmed it." (REJECT — delegated ≠ direct.)
- "No vision = Visual Media Skill refuses every task." (REJECT — manifest / metadata / delegation / user-review workflows remain possible.)

---

## Failure / PARTIAL handling summary

| Scenario | Action |
|---|---|
| Canonical logic correct but visual evidence unavailable | Mark PARTIAL Type A; use USER-SUPPLIED DESCRIPTION + technical inspection + Asset Manifest. |
| Canonical logic correct but runtime unavailable | Mark PARTIAL Type A; use `PROTOTYPE CODE` evidence label. |
| Canonical logic depends on evidence that should have been requested | Mark PARTIAL Type B; route visual QA / runtime verification. |
| Generated content requires QA | Route to capable Agent / user / fallback. Do NOT self-certify. |
| Hard-rule failure (e.g. "I read dimensions therefore I know the subject") | FAIL; adversarial test must catch. |

---

## End of contract
