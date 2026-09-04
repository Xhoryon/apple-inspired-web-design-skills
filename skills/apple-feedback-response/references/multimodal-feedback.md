# Multimodal Feedback

Coordination between feedback channels: visual, text, motion, haptic, audio. The Skill's rule: critical meaning must not depend on one channel.

## Channel inventory

| Channel | Use | Platform notes |
|---|---|---|
| **Visual** | icon, color, shape, layout | primary channel on all platforms |
| **Text** | explicit message | primary channel for accessibility |
| **Motion** | animation, transition | owned by `apple-motion-physics`; the Skill decides **why** motion is used as feedback |
| **Haptic** | tactile pulse | iOS / iPadOS / watchOS / macOS (trackpad) |
| **Audio** | sound, alert, spoken voice | iOS / macOS / Web (with caveats) |

## Why multimodal

Critical information must survive when one channel is unavailable:

- User has reduced motion preference → motion is reduced; visual + text + haptic remain.
- User has reduced transparency preference → visual surfaces are more opaque; layout is preserved.
- User is in a noisy environment → audio feedback may not be heard; haptic + visual remain.
- User is in a quiet environment → audio feedback may be inappropriate; visual + haptic + text remain.
- User has hearing loss → audio feedback is unreliable; visual + haptic + text remain.
- User has visual impairment → visual feedback is reduced; audio + haptic + text remain.
- Device has no haptic capability → haptic is unavailable; visual + text + audio remain.
- Browser does not support Web Vibration API on iOS Safari → visual + text + audio remain.

The Skill applies multimodal fallback: when one channel is unavailable, the others carry the meaning.

## Channel coordination

The Skill publishes the rule: channels should communicate a consistent event.

| Pair | Avoid |
|---|---|
| Visual success + error haptic | contradictory |
| Visual warning + success audio | contradictory |
| Haptic BEFORE the event meaningfully occurs | causal timing mismatch |
| Audio unrelated to the visual / haptic | unrelated sound |

The Skill rejects contradictory channel signals.

## Causal timing

The Skill publishes: causal timing matters. Haptic / audio should occur at the meaningful event, not before.

Examples:

- Button press → haptic AT the press, not before.
- Delete action → warning BEFORE the delete (preview), then success / acknowledgment AFTER (completion).
- Form validation error → error indicator AT the validation, not before.

The Skill does NOT publish exact timing numbers as canonical Apple rules. Any timing value is implementation heuristic, not Apple canonical.

## Accessibility media queries

The Skill honors Apple / W3C accessibility media queries:

- `@media (prefers-reduced-motion: reduce)` — reduce or remove animations.
- `@media (prefers-reduced-transparency: reduce)` — use more opaque surfaces.
- `@media (prefers-contrast: more)` — strengthen contrast.

The Skill applies these to multimodal feedback:

- Reduced motion → smaller / shorter / no animations; haptics may continue but at reduced intensity (the Skill defers timing to user setting).
- Reduced transparency → more opaque surfaces; visual feedback remains visible.
- Increased contrast → stronger visual contrast; text remains legible.

## Audio feedback (channels)

The Skill uses audio as a feedback channel. The Skill does NOT own general audio architecture:

- Music playback → owned by other Skills.
- Sound engine → owned by other Skills.
- Spatial audio → owned by other Skills.
- Media audio → owned by other Skills.

The Skill's audio feedback scope:

- **System sound effects**: short, semantic sounds for outcomes (success chime; warning sound; error sound).
- **Spoken feedback**: VoiceOver / accessibility narration.
- **Tone feedback**: short non-verbal audio cues.

The Skill does NOT require a specific audio engine. The Skill recommends:

- Use system-provided sounds where available (Apple's `AudioServicesPlaySystemSound` for system sounds).
- Use AVFoundation for custom audio where justified.
- Provide a way to disable audio feedback.

## Live regions on Web

The Skill uses WAI-ARIA live regions for status / error updates on Web:

- `aria-live="polite"` — status updates (announce after current narration).
- `aria-live="assertive"` — errors / urgent (interrupt current narration).

The Skill recommends marking elements appropriately:

- A progress indicator → `aria-live="polite"`.
- An error message → `aria-live="assertive"`.

The Skill does NOT mandate specific aria-* attributes; the Skill publishes the rule.

## What the Skill rejects

- **Haptic-Only Meaning** — relying on haptics alone.
- **Sound-Only Meaning** — relying on audio alone.
- **Visual-Only Meaning** — relying on color / icon alone (rejected by WCAG 1.4.1).
- **Text-Only Meaning** — text is the most accessible channel, but text alone is verbose when visual / haptic / audio would help.
- **Contradictory channels** — visual success + error haptic.
- **Causal timing mismatch** — haptic before the event.
- **Noisy feedback** — every channel firing on every action. Rejected.

## Coordination with other Skills

- **`apple-control-states`**: the control's state (idle / pressed / disabled / loading) is owned by `apple-control-states`. Feedback decides what the system communicates about state changes.
- **`apple-motion-physics`**: motion physics (spring / timing / interpolation) is owned by `apple-motion-physics`. The Skill decides **why** motion is used as feedback; `apple-motion-physics` decides **how** it moves.
- **`apple-modality-overlays`**: modal type (alert / sheet / popover) is owned by `apple-modality-overlays`. The Skill decides whether interruption is needed; `apple-modality-overlays` decides the modal type.
- **`apple-adaptive-structure`**: structural reflow is owned by `apple-adaptive-structure`. Feedback must survive reflow; the Skill publishes the rule.