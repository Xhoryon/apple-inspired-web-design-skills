# Apple Experience — Visual Authenticity Contract

**Date:** 2026-09-03
**Status:** INTERNAL — Post-v2.1.0 Visual Authenticity / Platform Composition / Input Fidelity Amendment
**Scope:** project-level cross-cutting contract (NOT Skill 14; not a per-Skill concern)

This contract prevents generic AI-generated visual language from replacing content-, platform-, brand-, and task-driven design.

---

## Purpose

Real-project evaluation exposed four recurring weaknesses:

1. AI-generated interfaces drift toward generic "AI aesthetic" (purple/cyan neon, arbitrary gradients, glow, colored glass, meaningless icons, sparkle/wand symbolism, ornamental progress bars / comparison graphics, dashboard-like decoration where direct content would be clearer).
2. Desktop / Tablet / Phone composition is not explicit enough.
3. Some scroll/drag interactions feel delayed or animation-driven rather than directly coupled to input.
4. Media presentation needs stronger canonical patterns (auto-advancing rails, partial next-card preview, direct drag/swipe, vertical-scroll-driven horizontal presentation, clean handoff back to normal vertical scrolling).

This contract is **cross-cutting** and applies across all Skills. Domain Skills retain local implementation ownership.

---

## Core Visual Authenticity principle

Hard rule: **Neutral-first, content-led color.**

Default interface chrome should generally begin from restrained, context-appropriate neutral structure unless one of these justifies additional color:

- product color
- brand identity
- semantic state
- content
- system material
- accessibility
- another real design reason

Do NOT establish: "Apple = black and white only." Apple/product experiences can use significant color.

The rejection target is: **arbitrary AI decoration.**

---

## AI Neon Syndrome

Anti-pattern. Examples:

- cyan → purple gradients by default
- blue / purple glow
- neon borders
- luminous orb backgrounds
- arbitrary gradient text
- colored-glass cards without semantic reason
- dark navy "AI dashboard" background merely to look futuristic

Expected: **REJECT BY DEFAULT** unless justified by:

- actual brand system
- product / media
- explicit user art direction
- meaningful state / content

Do not reject legitimate color.

---

## Gradient discipline

Hard rule: **Gradient must have a reason.**

Legitimate reasons:

- product artwork
- brand identity
- media / art direction
- physical / light representation
- intentional transition
- data encoding when genuinely appropriate

Reject: **gradient as generic premium / AI decoration.**

Especially avoid automatic: purple + cyan; blue + violet; neon spectrum presets.

---

## Glow discipline

Glow is NOT a default indicator of:

- premium
- intelligence
- selected
- interactive
- important

Do not use glow unless the visual / art direction actually requires it. Controls should communicate state through appropriate platform mechanisms, not "AI aura."

---

## Icon Semantic Gate

Hard rule: **Every non-decorative icon must have a clear recognizable semantic role.**

Prefer:

1. system / native symbol where appropriate
2. established domain symbol
3. text label where symbol is ambiguous

Reject:

- arbitrary egg icon
- random rocket
- random lightning
- generic sparkle
- magic wand
- AI orb
- unrelated colorful glyph
- one decorative icon per card merely for visual rhythm

Do NOT ban icons globally. Ban **meaningless iconography.**

---

## Sparkle-as-AI Default

AI capability does NOT automatically justify sparkles, magic wands, stars, or colorful gradient AI iconography. Use such symbols only when the actual product / brand / system convention supports them.

---

## Decorative Icon Soup

Reject the pattern where every card / section heading / metric / list row / feature receives its own decorative symbol. Use icons only when they materially improve recognition, navigation, status, action, or scanning.

---

## Visualization Earns Its Existence

Hard rule: **Data visualization must earn its existence.**

Before adding: progress bar; comparison bar; radial meter; gauge; score ring; multi-color chart — ask: does geometry / visual encoding make the information easier to understand? If no, prefer direct value, text, table, or simple aligned comparison.

---

## Fake Comparison Visualization

Anti-pattern. "18 hours vs 12 hours" does not automatically require two glowing progress bars. If direct values communicate better, use direct values. Do NOT invent percentages, scores, normalized bars, "best," stars, or arbitrary 0–100 metrics without real data semantics.

---

## Dashboardification

Turning ordinary product / content information into a dense dashboard of cards, metrics, colored chips, progress bars, icons, status dots — without task need. Professional apps may legitimately use dashboards; the anti-pattern is **dashboard structure without dashboard task.**

---

## Card Grid Everywhere

Every piece of information becoming a rounded card. Content hierarchy may instead use whitespace, alignment, typography, grouping, dividers, direct media composition. Cards must have a structural reason. Avoid duplicating existing Apple Template Syndrome guidance.

---

## Visual hierarchy over decoration

The UI should not need decorative effects to establish hierarchy. Prefer typography, spacing, alignment, scale, and content / media dominance before glow, gradients, decorative symbols, ornamental containers.

---

## Desktop / Tablet / Phone composition

Extend `apple-adaptive-structure` with a Tier 2 model for the three composition families. They are **composition families**, not rigid device detections.

### Desktop

Desktop MAY support:

- wider visual canvas
- restrained readable text widths
- split compositions
- persistent sidebar
- inspector
- pointer / keyboard affordances
- large cinematic media
- bounded sticky scenes
- horizontal media presentation

None are mandatory. Reject "desktop = fill every pixel." Whitespace remains legitimate.

### Tablet

Tablet is NOT large Phone and NOT small Desktop. Account for:

- touch-primary use
- optional trackpad / keyboard
- landscape and portrait
- resizable windows where platform supports it
- intermediate available space
- split views where task benefits
- sidebar / tab adaptation
- larger direct-manipulation regions

Avoid desktop-density controls merely because screen dimensions are large.

### Phone

Phone MAY favor:

- single-column editorial flow
- full-bleed media where appropriate
- simplified hierarchy
- touch-first controls
- bottom / inline actions where platform-appropriate
- direct horizontal media swipe
- reduced simultaneous information density

Do NOT merely scale Desktop down. Do NOT force desktop cross-axis cinematic mechanics onto Phone.

### Same experience, recomposed

Core principle: **Preserve task, hierarchy, and narrative intent — not literal geometry.**

Example: Desktop may be `text | product`; Tablet `text / product + detail`; Phone `text / product / detail`. This is valid adaptation, not design inconsistency.

### Three-family evaluation

For substantive interface work, require explicit evaluation of Desktop-like, Tablet-like, and Phone-like available space. This does NOT mean three separate implementations; it means do not declare responsive completion after checking Desktop only. Scope-proportional process still applies.

---

## Input-Coupled Motion

Strengthen `apple-motion-physics` and `apple-direct-manipulation` with the principle:

**Visual state should normally derive from current input / progress, not from a queue of destination animations.**

Conceptually: `input progress p → visual state f(p)`. Reject "input event → enqueue A → enqueue B → enqueue C" as a default pattern.

### "Follow the hand"

For direct manipulation, when finger / pointer moves, the manipulated visual should remain perceptually coupled to that movement. Reject: noticeable delayed catch-up; large arbitrary easing while finger is still moving; queued gesture animations; interaction that only begins after release — unless task intentionally requires indirect control. Do NOT invent a fixed millisecond threshold.

### Release / settle phase

After direct input ends, a short settle / snap may be appropriate. Motion Physics owns interpolation / retargeting / interruption / settling. Direct Manipulation owns gesture / control relationship. Visual Media owns narrative destination. Preserve boundaries.

---

## Auto-Advancing Peek Rail

Visual Media Layer C pattern (NOT Apple terminology):

Purpose: present a media sequence where one current item is dominant, part of the next item may remain visible, the rail can advance automatically when appropriate, users can directly navigate, content remains understandable without autoplay.

```
[ CURRENT CARD ][NEXT…]
```

A partial next item may communicate "more content exists beyond the current item." Do not prescribe a universal peek width. Do not create fake clipping when it harms composition.

### Auto-advance is optional

Hard rule: **Auto-advance is never required merely because a rail exists.** Use only for passive product highlights, editorial storytelling, or ambient visual exploration. Avoid where reading requires more time, comparison requires stable content, user interaction is primary, or accessibility / Reduced Motion argues against it.

### User interaction owns the rail

When the user drags, swipes, focuses, navigates, or otherwise takes control, automatic progression must not fight current interaction. Do not allow timer-driven movement during active drag.

### Direct rail manipulation

Touch should normally support direct swipe. Pointer contexts may support trackpad / drag / buttons / keyboard per platform / task. The rail must remain usable without autoplay.

### No animation queue in rails

Repeated user input must retarget from current visual state, not complete old queued slides first. If `A → B` auto transition begins and user immediately drags toward A, the transition yields / re-targets coherently — not finish B then animate back to A.

### Reduced Motion for auto rails

Respect Reduced Motion. Strategies: disable auto motion; restrained state transition; require manual progression; reduce large spatial travel. Preserve content access. Do not remove images / content.

---

## Bounded Scroll Handoff

Layer C pattern: normal vertical page → bounded cinematic / media section → vertical progress drives horizontal presentation → horizontal sequence completes → normal vertical page naturally continues. This is the explicit behavior needed for "down-scroll → left/right media → continue downward."

### Scroll Handoff, not Wheel Capture

Major invariant: **Prefer normal document scroll + bounded sticky / progress mapping over intercepting wheel / touch events and replacing native scrolling.** The experience principle is **preserve native-feeling scroll continuity.** Reject: preventDefault wheel capture; custom page inertia; locked trackpad behavior; delayed release after scene completion.

### Scroll allocation model

Section enters → vertical progress allocated to scene → horizontal media progress A → B → C → D → scene reaches final state → continued vertical progress exits section naturally. Reverse: previous section ← A ← B ← C ← D ← later page must remain coherent. Do not require fixed section heights or percentages.

### Seamless handoff

The transition from horizontal scene progression to normal vertical page movement should feel continuous. Reject: dead zone after final card; extra wheel gestures required to "unlock"; artificial pause at end; snap that fights current input; page suddenly jumping after horizontal sequence. This is a key "follow the hand" requirement.

### Boundary behavior

At first scene state, continued reverse progress should eventually return to previous vertical page. At last scene state, continued forward progress should naturally continue to next vertical section. Do not trap the user inside the media rail.

### Phone adaptation

Do NOT require Bounded Scroll Handoff on Phone. Possible Phone strategy: vertical page → native horizontal swipe rail → vertical page. Desktop may use vertical progress → horizontal cinematic → vertical progress. Choose based on clarity, touch behavior, viewport, narrative, user agency.

### Tablet adaptation

Tablet may use either bounded vertical→horizontal storytelling, direct horizontal touch rail, or hybrid editorial layout — depending on available space, orientation, window size, input device. Do not blindly copy Desktop.

### Cross-axis + direct manipulation coexistence

A media sequence may support Desktop (vertical progress drives horizontal) and Tablet/Phone (direct horizontal swipe) — two implementations of the same narrative sequence. Do not require identical mechanics.

---

## Ownership boundaries

- **Visual Media** owns: media sequence, narrative ordering, peek composition, Scene Choreography, desired horizontal progression, rail presentation.
- **Motion** owns: transition mechanics, interpolation, retargeting, interruption, settling.
- **Direct Manipulation** owns: drag / swipe interaction, direct gesture coupling.
- **Adaptive** owns: Desktop / Tablet / Phone structural adaptation.
- **Visual Authenticity Contract** (this document) owns: anti-AI visual constraints, neutral / content-led visual hierarchy, anti-decoration rules.

No ownership takeover.

---

## Anti-patterns (Layer C)

| Anti-pattern | Summary |
|---|---|
| AI Neon Syndrome | Generic purple/cyan/glow decoration without semantic reason. |
| Sparkle-as-AI Default | Sparkle / wand / star iconography without brand / system convention. |
| Decorative Icon Soup | Every card / section / metric receives its own symbol. |
| Fake Comparison Visualization | Two values turned into glowing progress bars without need. |
| Dashboardification | Dense dashboard of cards / metrics without dashboard task. |
| Card Grid Everywhere | Every piece of information becomes a rounded card. |
| Input Lag Theatre | Delayed catch-up, large easing during finger movement. |
| Animation Queue Interaction | Repeated inputs queue stale animations instead of retargeting. |
| Scroll Unlock Delay | Final scene requires extra wheel gesture to release vertical scroll. |
| Media Rail Fighting the User | Autoplay continues during active drag / swipe. |

---

## Current Apple evidence boundary

Apple's guidance supports adaptive layout across contexts, visual hierarchy, progressive disclosure, and partial visibility as a possible cue to more content. Do NOT upgrade that into claims that Apple mandates autoplay rails, horizontal scroll narratives, black-and-white-only palettes, specific carousel timings, specific peek widths, or specific breakpoints. Auto-Advancing Peek Rail and Bounded Scroll Handoff are Skill-owned patterns unless specific observational evidence is explicitly labeled.

---

## No fabricated numbers

Do NOT canonicalize autoplay seconds, drag thresholds, snap velocity, peek width, sticky height, breakpoint pixels, transition duration, or spring values without legitimate evidence / task need. Use qualitative guidance. Implementation may choose values contextually.

---

## Hard invariants (durable, project-level)

1. Neutral-first, content-led color.
2. Arbitrary AI decoration rejected by default.
3. Gradient and glow must have a reason.
4. Icons must have a recognizable semantic role.
5. Visualization must earn its existence.
6. Desktop / Tablet / Phone are composition families, not device detection. Preserve narrative intent, not literal geometry.
7. Tablet is NOT large Phone and NOT small Desktop.
8. Phone does NOT merely scale Desktop.
9. Input-coupled motion: visual state derives from current input, not queued animations.
10. Auto-advance is optional; user interaction owns the rail.
11. Bounded Scroll Handoff is preferred over wheel capture.
12. No fabricated breakpoint or timing numerology.

---

## Failure modes this contract prevents

- "Apple style means use a purple-to-blue gradient hero." (REJECT.)
- "AI feature should use a sparkle icon by default." (REJECT.)
- "Add an egg icon to make the feature card less empty." (REJECT.)
- "Turn two simple values into colorful progress bars." (REJECT when visualization adds no meaning.)
- "Apple means only black and white; remove product color." (REJECT.)
- "Tablet is just Desktop at 75% scale." (REJECT.)
- "Phone should keep the same two-column composition." (REJECT where inappropriate.)
- "Queue every wheel event as a 500ms animation." (REJECT.)
- "Autoplay should continue even while the user is dragging." (REJECT.)
- "The horizontal scene is complete; require one more wheel gesture before unlocking vertical scroll." (REJECT.)
- "Use preventDefault on all wheel input to guarantee cinematic behavior." (REJECT as default.)
- "Every horizontal gallery must auto-advance." (REJECT.)
- "Every gallery should expose 20% of the next card." (REJECT fixed numerology.)
- "Desktop horizontal story must stay horizontal on Phone." (REJECT.)
- "Any sticky horizontal scene is scroll hijacking." (REJECT.)
- "Because auto-advance is visually impressive, enable it under Reduce Motion unchanged." (REJECT.)