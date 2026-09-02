# Apple-inspired Anti-pattern Library

This is the explicit list of "fake Apple" / "fake Liquid Glass" mistakes the agent must detect and fix.
Loaded by `apple-design-audit` and referenced by all other skills in the pack.

Each entry follows: **Symptom → Why it fails → How to detect → Preferred correction.**

---

## 1. SaaS Purple Gradient

**Symptom:** Backgrounds or hero sections use a linear-gradient from `#6c5ce7`-ish purple to `#3b82f6`-ish blue, often with a glow.

**Why it fails:** Apple does not use multi-hue gradients in product UI. Purple-blue is the universal "I made this with a 2020 SaaS template" signal.

**How to detect:**
- grep `linear-gradient(... purple|blue|indigo|violet...)` in CSS.
- Look for `#6[0-9a-f]{2}\s*#?` paired with `#3[0-9a-f]{2}` or similar.
- Visual: hero background has visible hue shift between two saturated colors.

**Correction:** Use a single neutral background (off-white in light, near-black in dark). If you need color, bring it from a single accent or from product imagery.

---

## 2. Bento Everything

**Symptom:** Every section is a bento grid: rounded rectangles of varied sizes with internal padding, internal padding again, and an internal grid for "features".

**Why it fails:** Bento is a real layout pattern Apple uses occasionally, not a default. A page of bentos is a SaaS landing-page cliché.

**How to detect:**
- Count sections where the dominant block is "rounded rectangles in a grid" — if > 50% of sections, it is bento-everything.

**Correction:** Vary section types. Use full-bleed image sections, pinned storytelling, comparison tables, quote blocks, etc. (See `apple-web-composition`.)

---

## 3. Card Everywhere

**Symptom:** Every paragraph, every feature, every list item is wrapped in a card with rounded corners and shadow.

**Why it fails:** Cards are containers, not decoration. Card-soup hides hierarchy and makes the page feel like a CRM dashboard.

**How to detect:**
- Visual: most non-hero content is inside a bordered/shadowed surface.
- grep for `.card` usage; if applied to > 70% of content blocks, fail.

**Correction:** Use cards sparingly — for grouped related content, image+caption blocks, distinct product tiles. Most content should sit on the page surface.

---

## 4. Pill Everywhere

**Symptom:** Every button is `border-radius: 9999px`. Every chip. Every tag. Every input.

**Why it fails:** Pill is a rhythm choice, not a default. All-pill collapses button hierarchy and makes the page feel like a tag cloud.

**How to detect:**
- grep `border-radius:\s*9999`; if > 80% of buttons use it, fail.

**Correction:** Use 8–14 px radius for most buttons. Pill only for hero CTAs or where the design rhythm specifically calls for it.

---

## 5. Glass Everywhere

**Symptom:** Body text containers, every card, every section, and the page background all use `backdrop-filter`.

**Why it fails:** Glass is for the interaction layer. Glass-everywhere removes content/interaction separation and tanks performance.

**How to detect:**
- Count persistent `backdrop-filter` declarations. Threshold: 3 at Level 1, 1 at Level 2+, 1 at Level 3.
- Visual: the page reads as "translucent over translucent".

**Correction:** Glass belongs on floating navbar, toolbar, segmented control, sheet, popover, media controls. Body, cards, articles stay solid.

---

## 6. Blur = Liquid Glass

**Symptom:** A surface is `background: rgba(255,255,255,0.4); backdrop-filter: blur(12px); border: 1px solid rgba(255,255,255,0.3); border-radius: 24px;` — and nothing else.

**Why it fails:** This is frosted glass / glassmorphism baseline. It fails Liquid Glass on translucency (no adaptivity), refraction (no displacement), specular (no dynamic highlight), depth (no separation from content), dynamics (no motion), and morphing (no state).

**How to detect:**
- Visual + grep: a glass surface whose only Liquid Glass dimension is translucency. Flag as `FAKE LIQUID GLASS / FROSTED GLASS ONLY`.

**Correction:** Either add real Liquid Glass dimensions (refraction, specular, adaptivity, dynamics) or call it frosted glass and stop calling it Liquid Glass.

---

## 7. Giant Empty Space

**Symptom:** A section that is > 80 vh tall and contains less than 5% content by area — usually with one centered quote and a CTA, or nothing at all.

**Why it fails:** Whitespace is structural, not decorative. Big empty sections waste viewport and break narrative pacing.

**How to detect:**
- Measure section height vs. content density. If a section is > 80 vh with < 5% content, fail.

**Correction:** Either reduce the section height, add content (image, supporting text, secondary CTA), or merge with adjacent sections.

---

## 8. Excessive Rounded Corners

**Symptom:** Every element on the page has `border-radius: 20` or higher. Cards, buttons, inputs, images, icons, even dividers.

**Why it fails:** Apple uses concentric corners (outer > inner). Uniform-large radius looks plastic and form-over-function.

**How to detect:**
- Visual: every rounded element has the same large radius.
- grep `border-radius: 2[0-9]px`; if > 70% of elements use 20+ px, fail.

**Correction:** Vary radius by role. Use 8–14 px for buttons, 16–22 px for cards, 24–28 px for sheets/windows, concentric for nested.

---

## 9. Random Glow

**Symptom:** `box-shadow: 0 0 40px rgba(120, 80, 255, 0.6)` or similar colored glows on cards, buttons, or icons.

**Why it fails:** Glow is decoration without function. Apple uses subtle shadow (low opacity, large blur, small offset), not colored glow.

**How to detect:**
- grep `box-shadow:.*[0-9]+px.*rgba\([0-9]+,\s*[0-9]+,\s*[0-9]+` with high saturation values.

**Correction:** Use ambient + key shadow at low opacity. Reserve colored glow for focus rings on accent actions, sparingly.

---

## 10. Icon Bubble Everywhere

**Symptom:** Every icon — even icons next to body text — is wrapped in a tinted circular background.

**Why it fails:** Icon bubbles are for primary icon buttons. Wrapping every icon makes the page feel like a control panel.

**How to detect:**
- Visual: > 60% of icons are inside a tinted circle.

**Correction:** Most icons stand alone or sit inline with text. Icon bubbles only for primary icon buttons.

---

## 11. Generic Dashboard Hero

**Symptom:** Left half: headline + supporting text + 2 CTAs. Right half: a stylized dashboard mockup with charts, sidebars, fake data.

**Why it fails:** This is the SaaS template default. Apple product pages lead with the product, not a fake UI.

**How to detect:**
- Visual: hero is a 50/50 split with a dashboard mockup on the right.

**Correction:** Hero carries the product (image, 3D render, full-bleed photo, or a strong type composition). A dashboard mockup is only appropriate when the dashboard is the actual product.

---

## 12. Every Section Looks the Same

**Symptom:** Three or more consecutive sections follow the pattern: section title + supporting text + 3 (or 4) feature cards in a row.

**Why it fails:** Identical structure stacked creates a slide-deck feel and removes narrative rhythm.

**How to detect:**
- Visual: 3+ consecutive sections with "title + 3 cards" pattern.

**Correction:** Vary section types — full-bleed image, pinned storytelling, quote, comparison table, etc. (See `apple-web-composition`.)

---

## 13. Excessive Scroll Animation

**Symptom:** Every element fades up 20 px when it enters the viewport. Reveal-on-scroll is applied to > 4 elements per viewport.

**Why it fails:** Reveal-on-scroll is for emphasis. Reveal-everything is decoration that tires the eye.

**How to detect:**
- grep `IntersectionObserver`; if wired to > 10 elements, fail.
- Visual: scroll the page; everything cascades in.

**Correction:** Reveal one element per section. Most content should be present, not animated in.

---

## 14. Tiny Gray Text

**Symptom:** Body text below 16 px on mobile, or secondary text at opacity 0.3 / color `#999` on white.

**Why it fails:** Apple keeps body at 17 px on phones. Greying out secondary text makes it unreadable.

**How to detect:**
- Visual: body text looks small or washed out.
- DevTools: computed font-size on `body` or `p` < 16 px.
- DevTools: computed color contrast < 4.5:1.

**Correction:** Body 17–19 px desktop / 16–17 px mobile. Secondary text: smaller size, ~60% contrast — never < 4.5:1.

---

## 15. Glass-on-Glass

**Symptom:** A translucent floating navbar with translucent buttons, sitting over a translucent image header.

**Why it fails:** Stacking translucent layers destroys legibility and visual hierarchy. Each layer costs GPU.

**How to detect:**
- Visual: content behind a floating glass control is itself translucent or a busy image.

**Correction:** One floating glass layer over solid content. If the content is busy, the glass surface needs a scrim — or the content should be a solid surface.

---

## 16. Fake Apple Navbar

**Symptom:** A navbar with a logo that resembles Apple's bitten apple, or a logo placeholder saying "Apple" / "MacBook" / "iPhone" instead of the user's brand.

**Why it fails:** Trademark, brand dilution, and a tell that the agent didn't understand Apple-inspired — it copied Apple.

**How to detect:**
- Visual: navbar logo is an apple silhouette or has the user's product masquerading as Apple's.

**Correction:** Use the user's brand. The skill is Apple-inspired, not Apple-cloning. The user's logo or text mark goes there.

---

## 17. Copying Apple Instead of Understanding

**Symptom:** Page uses Apple's marketing copy ("The thinnest product we've ever made."), product names ("iPhone", "MacBook Pro"), or photography pulled from Apple's site.

**Why it fails:** Trademark, copyright, and the actual purpose of the pack — which is to teach the **principles** (hierarchy, restraint, typography, material) so the user's brand keeps its identity.

**How to detect:**
- grep for Apple product names, hero copy patterns like "All-new.", "The best [noun] we've ever made."

**Correction:** Apply the principles — restrained typography, generous space, content-led composition, restrained color, restrained motion. Use the user's product, copy, and imagery.

---

## 18. Desktop-only Design

**Symptom:** Mobile screenshot reveals a shrunken desktop layout — tiny text, horizontal scroll, overlapping elements, fixed-width containers.

**Why it fails:** Mobile is not a shrink. It is a redesign.

**How to detect:**
- Always capture mobile screenshots in the audit. Compare to desktop.

**Correction:** Mobile-first (or mobile-equally) composition. Stack columns. Reduce hero scale. Edge-to-edge media. Mobile navbar variants (bottom tab bar).

---

## 19. Performance-heavy Glass

**Symptom:** Page has > 5 persistent `backdrop-filter` surfaces, no fallback for `prefers-reduced-transparency`, large blur radius (≥ 30 px), or shader-glass applied to multiple elements.

**Why it fails:** Glass costs GPU. Performance-heavy glass tanks battery, frame rate on low-end devices, and Lighthouse scores.

**How to detect:**
- Lighthouse mobile run.
- DevTools Performance panel: scroll the page and watch for dropped frames.
- grep `backdrop-filter` and `feDisplacementMap` / `WebGL`; count instances.

**Correction:** Cap persistent backdrop-filter at 3. Provide solid fallback. Reduce blur radius on mobile. Cap shader glass at one surface per page, downsample the framebuffer, pause on idle/hidden.

---

## Critical vs warning

The audit marks these as **critical** (block ship) and **warning** (note, fix when possible):

**Critical** (zero allowed):
- #1 SaaS Purple Gradient (when used as page background or hero background)
- #5 Glass Everywhere
- #6 Blur = Liquid Glass (when the work claims to be Liquid Glass)
- #15 Glass-on-Glass
- #16 Copying Apple
- #20 Brand Erasure
- #24 Invisible Glass (covers both the "invisible over solid backdrop" symptom and the former #15 Low Contrast Glass symptom)
- #25 Mobile-Only-Forgotten Touch Targets (15+ controls below 44×44 on a mobile demo)

**Warning** (fix when possible):
- All others. Each contributes to checklist score and visual quality.

---

## 20. Brand Erasure

**Symptom:** After an "Apple-inspired" build, the original brand identity is gone — the page reads as an Apple page with someone else's logo pasted in.

Examples:

- A music-streaming product with Spotify-style content loses its green brand color and gets Apple's blue.
- A luxury fashion product loses its serif typography and adopts SF Pro.
- A banking product loses its enterprise gravitas and becomes "playful" because Apple is.
- A Chinese product loses its character and gets English-only or generic-Western typography.

**Why it fails:** Apple-inspired is a **quality bar**, not a brand replacement. The principles (hierarchy, restraint, motion discipline, material discipline) are transferable. The brand identity is not.

**How to detect:**

- Did the page's accent color change? Probably yes if brand was erased.
- Did the typography change to system-stack-only? Probably yes.
- Did the voice / copy tone change? Look at headlines — are they more "elevated / minimal" than the brand's actual voice?
- Compare before/after color swatches and headlines side by side.

**Correction:** Apple-inspired is **a layer on top** of the existing brand, not a replacement.

- Keep the brand's primary accent.
- Keep the brand's typography if it has one; pair with system stack for UI.
- Keep the brand's voice in copy.
- Apply Apple-quality restraint, hierarchy, motion, and material discipline **within** that brand.

---

## 21. Apple Template Syndrome

**Symptom:** Every Apple-inspired page converges on the same recognizable template:

- White / off-white page background
- Giant centered display headline
- Floating glass-capsule navbar
- Image-led hero with rounded media container
- Comparison table with one highlighted column
- Eyebrow uppercase above every section title
- Blue CTA

**Why it fails:** The pack's principles are general (hierarchy, restraint, motion, material). The output should not be the same template every time. Different products have different information architectures, different emotional registers, different brand voices. If the page always looks like Apple.com, the agent has not been thinking.

**How to detect:**

- Build the same product three times with three different products/brands. If all three outputs are visually similar, this anti-pattern is present.
- Does the hero always have a centered giant headline? Probably yes if syndrome is present.
- Does the navbar always look the same? Probably yes.
- Does the page always use the same section sequence (hero / image / features / compare / footer)?

**Correction:**

- Apply the principles to the specific product. Different products warrant different compositions.
- Editorial layouts (text-heavy, asymmetric) are also Apple-inspired.
- Dark product launches are also Apple-inspired.
- Dense technical product pages are also Apple-inspired.
- Media-heavy experiences are also Apple-inspired.

The pack describes **principles**, not **templates**. If your output is a template, you have not applied the principles.

---

## 22. Fake Optical Physics

**Symptom:** Surface has decorative elements that *look* like lens optics but are not actually simulating any optical behavior. Examples:

- Static gradient highlight that doesn't follow the pointer — labeled "specular response".
- Box-shadow with a bright color labeled "refraction".
- Rainbow edge tint labeled "chromatic aberration".
- Noise texture added to a `backdrop-filter` surface labeled "lensing".

**Why it fails:** Real Liquid Glass has dynamic, physics-derived behavior. Fake optical physics is decoration that misleads the viewer (and other agents reading the code) into thinking the surface has more capability than it does. It is the **opposite** of honesty in design.

**How to detect:**

- Read the CSS. Is there actual `feDisplacementMap` (real displacement)? Or just gradients?
- Move the pointer. Does the highlight track? If not, it's not specular response.
- Watch a state transition. Does the surface morph continuously? If not, it's not morphing.
- Is there JS that adapts to the backdrop luminance? If not, it's not adapting.

**Correction:**

- Either implement the optical behavior for real (SVG displacement for refraction, pointer-tracked gradient for specular, JS sampling for adaptivity).
- Or remove the labels / CSS comments that claim these behaviors exist.
- A surface that is just `backdrop-filter + border + shadow` is **Frosted Glass**, not Liquid Glass. Label it honestly.

---

## 23. CJK Typography Blindness

**Symptom:** SF-Pro-tuned typography rules are applied directly to Chinese / Japanese / Korean text without adjustment:

- `letter-spacing: -0.035em` on Chinese display (squishes characters).
- `line-height: 1.05` on Chinese display (cramped).
- `font-weight: 600` on Chinese body (heavy).
- Body 14 px on mobile (illegible).
- Mixed CJK + Latin without inter-script spacing.
- Line length measured in `ch` (meaningless for CJK density).

**Why it fails:** CJK characters have a uniform square bounding box and consistent stroke density. English rules do not transfer directly. The pack's trigger phrases include Chinese — Chinese products are in scope.

**How to detect:**

- `grep "letter-spacing: -" apple-*/references/*.md`. If applied to a Chinese build, anti-pattern.
- Check Chinese display font-weight ≥ 600 — too heavy.
- Check Chinese body line-height ≤ 1.5 — too tight.
- Check body font-size ≤ 15 px on mobile.

**Correction:** See `apple-design-foundations/references/cjk-typography.md`.

- Chinese display: `letter-spacing: 0` (or slightly positive).
- Chinese display line-height: 1.20–1.40.
- Chinese display font-weight: 500 (use 600 only for very short hero).
- Chinese body line-height: 1.6–1.8.
- Chinese body ≥ 16 px on mobile.
- Mixed CJK + Latin: explicit inter-script spacing.

---

## 24. Invisible Glass

**Symptom:** A glass surface is placed over a backdrop that has no optical information — flat color, near-flat color, off-white on off-white, single-tone gradient with no contrast. The `backdrop-filter` is technically active but the visual effect is invisible. The surface renders as a solid pill.

**Why it fails:** Glass is expensive. GPU cost, code complexity, fallback work — none of it pays back if the user cannot see anything happening. Worse, it creates a misleading demo: "look, our page has Liquid Glass!" — but the screenshot shows a solid white pill.

**How to detect:**

- Render the surface over the actual backdrop it will live above.
- Look at the result. Can you tell glass is active? If no, the glass is invisible.
- Compute the local contrast / variance of the backdrop under the surface. If variance is below a threshold, the surface is invisible.

**Correction:**

- **Rule:** Refraction requires optical information. Glass requires a backdrop with texture, contrast, geometry, or content variation.
- If the backdrop is flat, **don't use glass**. Use a solid floating control with subtle shadow.
- If glass is required (e.g. user explicitly asked for it), put it where it has something to refract: an image, a gradient field, structured visual content below.
- Always include a colorful or structured section in any Liquid Glass demo so the effect is visible.

---

**Related (formerly #15 Low Contrast Glass):** Body or label text on a translucent surface over a busy image, with no scrim or contrast protection. Glass only works if the text on it stays readable. Without contrast protection, the surface fails its actual job. Correction: scrim layer, theme-aware fill that goes opaque on busy backgrounds, or text shadow / backdrop stack. The detection rule (text contrast < 4.5:1 over worst-case background) is now part of this entry.
## 25. Mobile-Only-Forgotten Touch Targets

**Symptom:** A page is built at desktop first. Icon buttons are 36×36 (matches the desktop aesthetic). Segmented control buttons are 32 px tall. Footer links are 16 px line-height. The page passes desktop review and ships. On mobile, users cannot tap these controls.

**Why it fails:** Touch is the primary input on mobile. Sub-44×44 hit areas make the page unusable. The skill's own rule "Tap targets ≥ 44×44 px" is violated.

**How to detect:**

- `getBoundingClientRect()` on every interactive element at 390 px viewport.
- Flag any element with `width < 44 || height < 44`.
- Check `padding` (interactive hit area can extend beyond visual bounds if `padding` adds to the bounding box).

**Correction:**

- Icon button visual can be 24 px (icon size) inside a 44×44 interactive target.
- Segmented control button height: min-height 44 px (touch), or use padding to extend hit area to 44 px.
- Footer links: vertical padding 14 px or display `inline-block` with min-height 44 px.
- Use `padding` / wrapper / pseudo hit target — visual bounds and interactive hit area can differ.
- **Verify with bounding-box audit at the mobile viewport** before shipping.

---

## 26. Distortion Theater

**Symptom:** A "Liquid Glass" surface uses extremely high SVG displacement scale (≥ 10 px), exaggerated chromatic aberration, or excessive blur to look "more impressive". The result reads as water / jelly, not glass.

**Why it fails:** Real Apple Liquid Glass is **restrained**. Apple's native surfaces produce a subtle optical change — slight bending at edges, gentle highlight shift. Exaggerated displacement is decorative, not faithful. It also tanks performance (high-cost filters).

**How to detect:**

- `feDisplacementMap` `scale` attribute. If scale > 5 px, suspect.
- Multiple stacked filters with high intensity.
- Filter values designed to be visible at normal viewing distance (Apple's are barely perceptible).

**Correction:**

- Displacement scale ≤ 4 px on Level 2 surfaces.
- No chromatic aberration on Level 2 (Level 3 only, very subtle).
- Aim for "barely perceptible" optical change, not "wow look at this".
- Performance: high-cost filters drop frames on mid-tier mobile. Test there.

---

## When this library is incomplete

The audit may surface new failure modes not yet covered. Add new entries in the same format: symptom, why-fails, detection, correction. Then re-classify as critical or warning.
