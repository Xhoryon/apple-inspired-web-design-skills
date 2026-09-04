---
name: apple-visual-media-composition
description: Use when designing how visual media is planned, composed, presented, explored, and transitioned across an entire digital experience. Owns media role architecture, Hero choreography + state graphs, crop / focal-point / art direction, scroll cinematic + reversibility, interactive media explorer state, asset-feasibility gate, agent / tool capability gate, AI supplementation gate, system preview vs custom preview, Browse / Preview / Inspect / Compare / Work application modes, and reduced-motion narrative preservation. Applies to Native iOS / iPadOS / macOS / Mac Catalyst / visionOS / tvOS / watchOS and Web (Desktop / Touch / PWA). Do NOT use for typography (use stable apple-design-foundations), motion physics (use apple-motion-physics), drag / orbit / pinch mechanics (use apple-direct-manipulation), structural reflow (use apple-adaptive-structure), keyboard / focus / commands (use apple-keyboard-focus-commands), text input / editing / selection (use apple-text-input-editing), validation outcome / error communication (use apple-feedback-response), or component visual appearance (use stable apple-ui-components).
version: "0.8.0-dev"
license: MIT
---

# apple-visual-media-composition

Visual Media Systems, Composition & Cinematic Storytelling. Owns how visual media is planned, composed, presented, explored, and transitioned across an entire experience. Does NOT own typography, motion physics, drag mechanics, structural reflow, keyboard, text editing, or component appearance.

## When to use

Use when the task involves: media role / placement / prominence; Hero choreography or cinematic storytelling; asset feasibility for visual ambition; interactive media explorers; Browse / Preview / Inspect / Compare / Work application modes; system preview (Quick Look) integration; AI asset supplementation; responsive art direction; reduced-motion narrative preservation. Symptoms: "build a Hero section", "rotate the product", "show a comparison", "make this photo larger", "the image looks cropped", "scroll-driven media state".

Do NOT use for: typography (stable v1.1 `apple-design-foundations`); motion physics (`apple-motion-physics`); drag / orbit / pinch mechanics (`apple-direct-manipulation`); structural reflow (`apple-adaptive-structure`); keyboard / focus / commands (`apple-keyboard-focus-commands`); text input (`apple-text-input-editing`); validation outcome (`apple-feedback-response`); component appearance (stable v1.1 `apple-ui-components`).

## Core model (Skill-owned)

The Skill uses a **Whole → Detail → Whole** workflow (Layer C Skill-owned, NOT Apple HIG):

```
Project intent
      ↓
Experience architecture
      ↓
Visual Experience Blueprint
      ↓
Media architecture (Media Role Contract per section)
      ↓
Asset audit (AVAILABLE / MISSING / UNKNOWN)
      ↓
Feasibility
      ↓
Section / app-state planning
      ↓
Detailed composition / Hero storyboard
      ↓
Experience prototype
      ↓
User review
      ↓
Revision
      ↓
Whole-experience review
      ↓
Detailed implementation
```

This is **Scope-Proportional Process**: a small scoped request ("improve the image layout in this detail view") may proceed directly. A whole-product / whole-site / cinematic-product experience requires broader intake.

## Hard invariants

1. **System primitive first.** Use `QLPreviewController` / `QLPreviewPanel` / `quickLookPreview` / `UIImageView` / `NSImageView` / SwiftUI `Image` / `AsyncImage` / `VideoPlayer` before custom preview / composition code.
2. **Asset reality before visual ambition.** Presentation ambition must not exceed what assets can reliably provide. One front PNG cannot support authentic 180° rotation. CSS `rotateY` on a flat image is NOT a new physical viewpoint.
3. **Marketing media vs working media distinction.** Marketing / editorial may dominate viewport; cinematic-pace; crop intentionally. Working media prioritizes content fidelity; stable zoom / selection; supporting chrome; avoids theatrical interruption.
4. **Hero is optional.** A strong static composition is often better than a broken cinematic. Not every project needs a Hero section.
5. **Hero Choreography Brief precedes complex Hero.** If the user has not specified choreography for a complex Hero, ASK. Convert natural-language choreography into Visual Beats. State describes what the picture communicates, not implementation technology.
6. **Visual Experience Blueprint precedes implementation.** For substantial whole-product work, the Blueprint describes overall media tone; light/dark progression; visual density; primary visual moments; editorial vs cinematic balance; interactive media locations; narrative rhythm; quiet/rest sections; platform adaptations.
7. **Media Role Contract per section.** Each section's media role (identity / proof / explainer / contextual / editorial / decorative / application / preview / inspection / comparison / working) drives scale, crop, interaction, prominence, fallback.
8. **Crop policy follows role.** Atmospheric may tolerate stronger crop; product proof must preserve key evidence; preview prioritizes identity recognition; inspection preserves content; working content does NOT silently crop meaningful data.
9. **Responsive art direction ≠ simple shrinking.** Different contexts may require different crop / focal point / alternate asset / alternate orientation / image-first vs text-first change. Coordinate with `apple-adaptive-structure`.
10. **User-owned state ≠ scroll-owned state.** User choices (variant, color, feature) persist across section transitions. Scroll-owned state (camera, scale, rotation) may reverse when user scrolls upward.
11. **Reduced motion preserves narrative.** Information ordering, explanation, destination remain. Only the motion path is reduced (key pose A → fade → key pose B → detail).
12. **Browse / Preview / Inspect / Compare / Work distinct.** Different priorities for each. Preview ≠ Inspect; Compare ≠ Inspect; Work avoids marketing spectacle.
13. **AI supplementation requires user permission + capability gate + fidelity gate.** Do NOT silently generate missing product views. AI-generated sequences are NOT animation-ready automatically. AI-generated technical content (unseen geometry / features) must NOT be fabricated.
14. **UNKNOWN ≠ MISSING.** Before concluding the user does not possess an asset, ASK.
15. **Three Editorial Presentation Modes distinct.** Static / Editorial (Mode A); Interactive Media Explorer (Mode B); Cinematic Storytelling (Mode C). NOT an ordered maturity ladder. A design is not "better" because it reaches Cinematic.
16. **Three evidence layers explicit.** Layer A (Apple official / W3C / SDK / Web standards); Layer A (Apple public-site OBSERVATIONAL / VERSION-SPECIFIC); Layer B (Skill synthesis); Layer C (Skill-owned vocabulary). Never collapsed. Apple public-site observations are NOT Apple HIG canon.
17. **Paraphrase by default.** Apple HIG portal pages return JS-rendered SPA shells; verbatim HIG text is UNVERIFIED in this session. Apple public-site observations are recorded as OBSERVATIONAL / VERSION-SPECIFIC, never as Apple HIG law.
18. **No fabricated Apple APIs.** Every symbol passes the scanner's `VISUAL_MEDIA_PRESENTATION` allowlist. No `XRMediaStreamCoordinator`, `XRMediaCompositionView`, `VisualMediaSceneView`, `UIHeroCompositionView`, `NSHeroCompositionView`, `MediaHeroChoreographer`, `UIVisualMediaPreviewController`, `SensoryHeroView`, etc.
19. **Visual Inspection Capability Gate.** Before claims involving focal point / crop / visual consistency / product accuracy / AI asset review / sequence continuity, determine whether the current Agent has semantic visual-inspection capability. If not: use Asset Manifest + `USER-SUPPLIED DESCRIPTION`; use `DELEGATED VISUAL INSPECTION` from a capable Agent; ask for clarification; use metadata-only reasoning where sufficient; or mark the decision unverified / PARTIAL Type A. The project-level `APPLE-EXPERIENCE-CAPABILITY-EVIDENCE-CONTRACT.md` is the binding rule; do NOT upgrade visual evidence provenance through reading another Agent's report.
20. **Scroll progress is independent from visual direction.** Scroll direction is an input / progress signal, not a required visual-motion direction. Vertical scroll may legitimately drive horizontal translation, scale, framing, rotation, opacity, mask / reveal, media-frame progression, text hierarchy changes, layout recomposition, background changes, annotations, or section transitions.
21. **Scene Choreography is broader than Hero.** A Scene may include media, typography, background, layout, annotations, product / object, framing, and controls where appropriate. Cinematic composition does not require video or 3D — it may use typography, static images, masks, scale, layout recomposition, and subtle parallax.
22. **Cross-axis storytelling must preserve user agency.** Mapping vertical progress to horizontal / cinematic presentation must NOT unnecessarily hijack normal scrolling. The user should retain understandable page progress, the ability to reverse, the ability to continue beyond the scene, and reasonable escape from long cinematic regions. Reject endless-feeling pinned sections and wheel / trackpad interception that breaks expected page navigation.
23. **Scroll-owned scene state remains reversible.** Scrubbed narrative state (A → B → C → D) should reverse coherently (D → C → B → A) whether presentation is vertical, horizontal, scale, rotation, typography, or layout recomposition. User-owned state (chosen variant / color / feature) survives reverse scroll.
24. **Video semantic conservatism.** If the current Agent cannot semantically inspect video, treat the video's visual content as `UNKNOWN` (Opaque Media mode) unless trustworthy semantic evidence exists. Metadata (duration / dimensions / codec / frame count) does NOT establish shot content / camera movement / visual continuity. Generated video does NOT self-certify visual quality. Playback capability ≠ cinematic suitability ≠ scroll-scrub suitability.

## Cross-Cutting Contract Hook

Visual Authenticity Contract (`APPLE-EXPERIENCE-VISUAL-AUTHENTICITY-CONTRACT.md`) applies to this Skill for visual composition, autoplay rails, peek rails, Bounded Scroll Handoff, and decorative-presentation decisions. The contract is a cross-cutting constraint; this Skill does NOT inherit its definitions wholesale. Local Visual Authenticity consequences: neutral-first / content-led color; no arbitrary AI neon / glow; no meaningless icons; no fake visualization; autoplay optional and yields to user input; Bounded Scroll Handoff preferred over wheel capture; no fabricated numbers.

## Routing integration

The orchestrator (`apple-experience-design`) adds a row for the new Skill. **Selective load rule:** the row applies ONLY when the task explicitly involves visual media composition (media role / Hero / cinematic / explorer / application media modes). For ordinary state / motion / structural / material / text / focus / validation tasks, the orchestrator routes WITHOUT loading `apple-visual-media-composition`.



Discriminator checklist (the orchestrator uses these to gate loading):

- Is the task about media role / prominence / placement? If yes, load.
- Is the task about Hero / cinematic storytelling? If yes, load.
- Is the task about interactive media explorer (variant / color / feature / viewpoint)? If yes, load.
- Is the task about application media modes (Browse / Preview / Inspect / Compare / Work)? If yes, load.
- Is the task purely about typography / text visual hierarchy? If yes, do NOT load.
- Is the task purely about motion physics (spring / timing / interpolation)? If yes, do NOT load.
- Is the task purely about structural reflow? If yes, do NOT load.
- Is the task purely about text input / editing inside a media inspector? If yes, do NOT load.
- Is the task purely about loading / failure state visualization? If yes, do NOT load; coordinate with `apple-feedback-response`.

## Per-platform visual media API surface (summary)

| Platform | Image | Async image | Video | 3D / Model | Preview |
|---|---|---|---|---|---|
| iOS / iPadOS | `Image` (SwiftUI), `UIImageView` | `AsyncImage` (iOS 15+) | `VideoPlayer` (iOS 14+) | `RealityView` (iOS 18+) | `quickLookPreview` / `QLPreviewController` |
| macOS | `Image` (SwiftUI), `NSImageView` | `AsyncImage` (macOS 12+) | `VideoPlayer` (macOS 11+) | `RealityView` (Mac Catalyst 18+) | `QLPreviewPanel` |
| watchOS | `Image` | `AsyncImage` (watchOS 8+) | `VideoPlayer` (watchOS 7+) | n/a | system preview where applicable |
| tvOS | `Image` | `AsyncImage` (tvOS 15+) | `VideoPlayer` (tvOS 14+) | n/a | focus-driven preview |
| visionOS | `Image` | `AsyncImage` (visionOS 1.0+) | `VideoPlayer` (visionOS 1.0+) | `Model3D` (visionOS 1.0+) / `RealityView` (visionOS 1.0+) | spatial preview |
| Web | `<img>` / `<picture>` | lazy load via `loading="lazy"` + `IntersectionObserver` | `<video>` with `poster` | n/a (WebGL for 3D) | Web preview = browser-native |

## Reference catalog (Tier 2)

- `references/professional-media-workflow.md` — Whole → Detail → Whole; scope-proportional process; Visual Experience Blueprint.
- `references/media-role-architecture.md` — Media Role Contract; visual dominance; static / explorer / cinematic modes; Browse / Preview / Inspect / Compare / Work.
- `references/static-media-composition.md` — large centered media; split text/media; full-bleed vs contained.
- `references/focal-point-crop-art-direction.md` — focal point; protected region; safe text; crop tolerance; responsive art direction.
- `references/asset-feasibility-authenticity.md` — Asset Inventory; Asset Capability Ladder; Presentation Ambition; agent / tool capability gate; AI supplementation gate; AI fidelity gate; generated-asset review.
- `references/visual-inspection-evidence.md` — technical vs semantic inspection; no-vision Agent workflow; Asset Manifest; Delegated Visual Inspection; Generation ≠ Inspection; Visual Evidence Ladder (VE0–VE3); required escalation / fallback; never bluff capability / perception.
- `references/anti-patterns.md` — visual-media-specific anti-patterns (Apple Media Template; Cinematic Overreach; Asset Bluffing; Fake Viewpoint; WebGL Prestige; Silent AI Asset Synthesis; AI Sequence Consistency Blindness; One-Way Scroll Cinematic; Scroll Resets User State; Reduced-Motion Narrative Collapse; Marketing-Site-Inside-an-App; Preview/Inspection Collapse; Destructive Crop; Visual Climax Everywhere).
- `references/hero-cinematic-storyboarding.md` — Hero Choreography Brief; Visual Beats; Hero State Graph; "establish → transform / reveal → focus → inspect / emphasize → transition".
- `references/scroll-cinematic-storytelling.md` — scroll → narrative state → media / camera / text state; reversibility; reduced motion preservation.
- `references/interactive-media-explorer.md` — user choice → persistent user media state → visual representation → optional explanation; user-owned vs scroll-owned state.
- `references/application-media-modes.md` — Browse / Preview / Inspect / Compare / Work; system preview (Quick Look) primitive first.
- `references/media-loading-failure.md` — `AsyncImage` phases; `<img>` onload / onerror; `IntersectionObserver`; aspect ratio reservation; state continuity.
- `references/media-capability-performance.md` — graceful capability ladder (3D → pre-rendered sequence → composed 2D motion → static key visual); performance constraints.
- `references/swiftui-native-media.md` — `Image` / `AsyncImage` / `VideoPlayer` / `quickLookPreview` / `RealityView` per-platform.
- `references/uikit-appkit-quicklook-media.md` — `UIImageView` content modes; `QLPreviewController`; `NSImageView`; `QLPreviewPanel`; per-platform primitive.
- `references/web-responsive-cinematic-media.md` — `<picture>` / `srcset` / `sizes` / `object-fit` / `object-position` / `aspect-ratio` / `prefers-reduced-motion` / live regions.
- `references/media-accessibility.md` — alt text; captions; audio descriptions; VoiceOver; reduced motion; multimodal alternatives.

## Process notes

- The Skill does NOT introduce a fabricated Apple API symbol. Every symbol is verified in DocC + installed SDK per the inventory.
- The Skill does NOT make verbatim Apple HIG claims (HIG portal pages return JS-rendered shells in this session).
- Apple public-site observations are recorded as OBSERVATIONAL / VERSION-SPECIFIC, never as Apple HIG canon.
- The Skill is verified by 24 functional tests (AE8-T1..T24) + 26 adversarial tests (AE8-G1..G26) + 14 router tests (AE8-R1..R14).
- The Skill coordinates with: `apple-motion-physics` (motion continuity, easing); `apple-direct-manipulation` (drag orbit / pinch); `apple-adaptive-structure` (responsive art direction); `apple-keyboard-focus-commands` (focus inside media explorers); `apple-control-states` (selected media state); `apple-feedback-response` (loading / failure status).