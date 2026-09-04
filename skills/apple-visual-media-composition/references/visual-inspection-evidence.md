# Visual Inspection & Evidence

This reference is the operational application of the project-level `APPLE-EXPERIENCE-CAPABILITY-EVIDENCE-CONTRACT.md` for visual-media work. It complements `references/asset-feasibility-authenticity.md` and is referenced by `SKILL.md` hard invariant #19 (Visual Inspection Capability Gate).

## Technical vs semantic inspection

| Dimension | Technical Asset Inspection | Semantic / Visual Inspection |
|---|---|---|
| Can be performed without visual perception? | yes | no |
| Examples | dimensions, format, alpha, EXIF, codec, frame count, file size, model file type | subject position, focal point, crop safety, composition, geometry consistency, lighting, image quality, animation continuity |
| Hard rule | May support many design decisions where content semantics are not material | Required before claiming visual / product decisions |
| Evidence label | `ASSET INSPECTION` (or `PROTOTYPE CODE` if the test verdict combines technical + implementation) | `DIRECT VISUAL INSPECTION` (only when the current Agent actually inspected pixels); `DELEGATED VISUAL INSPECTION` (when another capable Agent inspected and produced a traceable report); `USER-SUPPLIED DESCRIPTION` (when a professional supplied the description) |

**Never collapse these categories.** Reading dimensions does not equal understanding the image.

## When a decision requires visual evidence

| Decision category | Visual evidence required? |
|---|---|
| Subject position | yes |
| Focal point | yes |
| Safe crop | yes |
| Composition / hierarchy | yes |
| Image consistency (multi-view sequence) | yes |
| Product geometry / lighting | yes |
| Frame-to-frame continuity | yes |
| File dimensions | no (metadata only) |
| Aspect ratio selection | no (metadata only) |
| File size / format | no (metadata only) |

When the decision requires visual evidence and the current Agent cannot semantically inspect, route through:
- **Asset Manifest** supplied by user / professional.
- **Delegated Visual Inspection** by a capable vision Agent (report labeled).
- Ask the user.
- Use a safer default (contained framing, placeholder, wider safe-crop range).
- Mark `PARTIAL Type A` with explicit limitation.

## No-vision Agent workflow

When the current Agent cannot semantically inspect media:

1. **Inventory.** Use technical inspection (dimensions / format / frame count / codec).
2. **Classify.** Decide whether the decision requires visual evidence.
3. **Route.** If yes, use Asset Manifest + `USER-SUPPLIED DESCRIPTION`; use `DELEGATED VISUAL INSPECTION`; ask the user; or use a safer default.
4. **Implement.** Build the structural / responsive / interactive layer; reserve the actual visual treatment for the visual evidence step.
5. **Test.** Use evidence-typed tests with appropriate labels.
6. **Surface.** In the test verdict, mark the visual decision as PARTIAL Type A with explicit limitation. Do NOT PASS.

## Generation ≠ Inspection

Major invariant: **Generation capability does NOT imply verification capability.**

When the current Agent generates assets (image / video / 3D / multi-view), it MUST route visual QA through:
- capable vision Agent (`DELEGATED VISUAL INSPECTION`),
- user confirmation,
- or a lower-risk fallback.

Do NOT self-certify generated output. AI-generated multi-view is NOT automatically animation-ready.

## Inspection ≠ Generation

An Agent capable of visual inspection does NOT automatically have:
- image-generation capability,
- image-editing capability,
- video-generation capability,
- 3D-generation capability.

Keep these dimensions separate.

## Asset Manifest (recommended schema)

Use only where Scope-Proportional Process warrants it.

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

This is `USER-SUPPLIED DESCRIPTION` evidence unless the producing agent is identified as a `DELEGATED VISUAL INSPECTION` source.

## Visual Evidence Ladder

Project / Skill-owned Layer C taxonomy (NEVER Apple terminology):

| Level | Meaning |
|---|---|
| `VE0` | Metadata only. No semantic visual confirmation. |
| `VE1` | User-described. Semantic description supplied by user / client. |
| `VE2` | Delegated visual inspection. A capable vision Agent has inspected and documented. |
| `VE3` | Direct visual + relevant runtime verification. Strong direct visual / runtime evidence. |

Use the appropriate level; do not pretend a higher level.

## Hero / sequence implications

Scenario: Agent has `frame_001.webp` ... `frame_120.webp` and a GLB but lacks visual semantic capability.

| Allowed claim | Required evidence |
|---|---|
| 120 files exist with consistent technical properties | `ASSET INSPECTION` (technical) |
| Files are dimensionally consistent | `ASSET INSPECTION` (technical) |
| Smooth 180° product rotation | `DIRECT VISUAL INSPECTION` + `VE3` |
| Subject position consistent across frames | `DELEGATED VISUAL INSPECTION` + `VE2` minimum |
| Frame-to-frame continuity | `DIRECT VISUAL INSPECTION` + `VE3` |

Expected no-vision verdict wording:

> Technically sequence-shaped; visually unverified.

## Focal point implications

Without visual evidence, do NOT infer focal point from:
- filename,
- dimensions,
- aspect ratio,
- alpha channel,
- EXIF.

Use a professional Asset Manifest or user-supplied description. Otherwise ask / delegate / avoid relying on unknown focal positioning.

## Crop implications

When crop safety depends on where meaningful content lies and the Agent cannot see the media, do NOT silently choose a destructive crop. Safer options include:
- contained presentation,
- known safe crop from Asset Manifest,
- placeholder framing,
- user review,

until stronger visual evidence exists.

## Required escalation / fallback matrix

| Scenario | Required action |
|---|---|
| Asset exists; no vision capability | Asset Manifest OR Delegated Visual Inspection OR user-supplied description OR safer default |
| Asset missing; no vision capability | Ask; possibly AI supplementation (with permission) |
| Visual decision needed; no vision capability; no Asset Manifest; no Delegated Visual Inspection | Mark PARTIAL Type A; do not PASS; document as unverified |
| Generated content requires visual QA | Route visual QA to capable Agent / user / fallback |
| Visual evidence required for Hero / sequence continuity | Direct visual + runtime verification OR Delegated Visual Inspection + Asset Manifest |

## Demo / user-review pathway

A vision-unavailable coding Agent may still:
- build multiple layout variants,
- create placeholders,
- implement responsive structure,
- implement media states,
- expose variants for review,

and then a professional user / capable visual Agent can judge the result. This is legitimate. Do NOT imply "no vision = cannot perform Visual Media work at all." Instead: "no vision = narrower evidence authority."

## Failure modes this reference prevents

- "I can read image dimensions, therefore I know the product is centered." (REJECT.)
- "The filename is hero-right, therefore the subject is on the right." (REJECT.)
- "A different Vision Agent said it is center-right, so I can claim I visually confirmed it." (REJECT.)
- "I generated four views, therefore they are visually consistent." (REJECT unless visually reviewed.)
- "I can inspect the image, therefore I can generate the missing rear view." (REJECT.)
- "There are 120 same-size frames, therefore the animation is smooth." (REJECT.)
- "GLB parses successfully, therefore the realtime cinematic is visually verified." (REJECT.)
- "React prototype compiles, therefore Safari runtime is verified." (REJECT.)
- "SwiftUI code compiles, therefore native interaction was executed." (REJECT unless actually run.)
- "User says the focal point is left; label that DIRECT VISUAL INSPECTION." (REJECT.)
- "The Agent cannot see the media, so silently choose object-fit: cover." (REJECT if crop safety matters.)
- "No vision capability means Visual Media Skill must refuse every image task." (REJECT; metadata/manifest/delegation/demo workflows remain possible.)
