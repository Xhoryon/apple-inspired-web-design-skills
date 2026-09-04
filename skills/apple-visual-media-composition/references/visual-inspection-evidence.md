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

## Video Semantic Conservatism (Opaque Media mode)

Major Capability & Evidence extension for video. If the current Agent cannot semantically inspect video, treat the video's visual content as `UNKNOWN` unless trustworthy semantic evidence exists.

A video file being available proves **technical asset availability**, not understanding of its shots / content.

### Metadata-only knowledge of a video

Possible:
- duration
- dimensions
- codec
- frame rate
- file size
- audio-track presence
- container / format

These do **NOT** prove:
- subject position
- shot sequence
- camera movement
- product angle
- visual continuity
- transition point
- crop safety
- semantic timeline

### Opaque Media mode

For media whose technical existence is known but whose semantic visual content has not been inspected:

| Field | Value |
|---|---|
| technical | AVAILABLE |
| semantic visual content | UNKNOWN |

An Opaque Media asset may still be:
- stored
- referenced
- technically integrated
- shown as ordinary playback media

… but must NOT be the basis for detailed cinematic assumptions.

### Playback ≠ Cinematic Suitability ≠ Scroll-Scrub

Even if a video can technically play, the Agent may still not know whether it supports meaningful scroll-scrubbing. Potential concerns include hard scene cuts, discontinuous camera path, poor intermediate-frame meaning, seek behavior, codec / GOP characteristics, mobile support, and crop / framing changes. Do not overclaim what was not inspected / tested.

### Conservative default for non-multimodal Agents

If video semantics are unknown, prefer one of:

1. ask user for a shot description / storyboard
2. request a capable vision / video inspection
3. use the video as ordinary playback media only
4. choose a static / composed-motion alternative
5. build a placeholder prototype pending review

Do NOT automatically bind scroll progress to `video.currentTime` without independent grounding / review.

### Keyframe review (bounded)

If video understanding is unavailable but tooling can extract a SMALL representative keyframe set safely, keyframes may support bounded visual inspection. However:
- sampled frames may miss important transitions
- keyframes do NOT automatically prove smooth temporal continuity
- one inspected frame does NOT prove the whole video

Evidence must remain bounded.

### Generated video

Preserve: generation capability ≠ inspection capability. An Agent that can generate a video but cannot inspect it may NOT claim product consistency, camera continuity, temporal coherence, or transition quality. Require direct capable inspection, delegated inspection, user review, or a lower-risk fallback before treating the generated video as validated cinematic media.

### Metadata-to-Storyboard Hallucination

Adversarial rule: "The video is 8 seconds long, therefore 0–2 s is establish, 2–5 s rotation, 5–8 s detail." REJECT. Timeline meaning cannot be inferred from duration alone.

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
