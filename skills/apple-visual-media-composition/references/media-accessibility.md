# Media Accessibility

WCAG 2.2 + WAI-ARIA 1.2 + Apple accessibility. Hard rules for media composition.

## WCAG 2.2 Success Criteria (verified)

| SC | Title | Application |
|---|---|---|
| 1.1.1 | Non-text Content | alt text for images; decorative images marked appropriately |
| 1.2.1 | Audio-only and Video-only (Prerecorded) | alternative for audio-only / video-only |
| 1.2.2 | Captions (Prerecorded) | captions for prerecorded video with audio |
| 1.2.3 | Audio Description or Media Alternative (Prerecorded) | audio description for prerecorded video |
| 1.2.4 | Captions (Live) | captions for live video with audio |
| 1.2.5 | Audio Description (Prerecorded) | audio description for prerecorded video |
| 2.1.1 | Keyboard | all functionality via keyboard |
| 2.4.7 | Focus Visible | visible focus indicator on focusable elements |
| 4.1.2 | Name, Role, Value | programmatic name + role + value |

## WAI-ARIA 1.2 (verified)

- `aria-label` — accessible name.
- `aria-describedby` — description reference.
- `aria-labelledby` — label reference.
- `role="img"` — image role.
- `aria-live="polite"` / `aria-live="assertive"` — live region politeness.
- `aria-hidden="true"` — hide decorative content from screen readers.

## Apple accessibility

- VoiceOver / Voice Control: navigate the accessibility tree; reads label + value + hint.
- Increase Contrast: editor chrome increases contrast.
- Reduce Motion: prefers-reduced-motion honored.
- Reduce Transparency: prefers-reduced-transparency honored.
- Accessibility Inspector: validate accessibility tree.

## Reduced motion (CSS Media Queries + system)

- `@media (prefers-reduced-motion: reduce)` — replace animation with key pose / fade / static.
- Preserve narrative (feature ordering, explanation, destination) even when motion is reduced.
- Do NOT simply set `animation-duration: 0` if doing so destroys the visual story.

## Alt text

For each non-text content:
- Concise description of what the media is.
- Function (what the media does) when functional.
- Decorative images marked `alt=""` (or `role="presentation"` on Web).

## Captions + audio description

For video:
- Captions: spoken dialogue + relevant non-speech audio.
- Audio description: narration of important visual content not described in dialogue.

For interactive media (3D / explorer):
- Ensure the control surface is accessible (focus order, labels, ARIA roles).
- The visual exploration may not have direct equivalent; provide a non-visual alternative where appropriate.

## Multimodal alternatives

Critical meaning must not depend on visual alone:
- Important information delivered by video → captions / transcript.
- Important information delivered by image → alt text / long description.
- Important interaction → keyboard alternative.
- Important visual state → aria-live announcement.

## Reference

- `references/scroll-cinematic-storytelling.md` — reduced motion
- `references/web-responsive-cinematic-media.md` — Web primitives
- `references/professional-media-workflow.md` — workflow