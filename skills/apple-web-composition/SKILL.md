---
name: apple-web-composition
description: Use when designing any landing page, marketing page, product detail page, feature page, long-form sectioned web page, **dense productivity / dashboard / data-heavy application surface, or editorial / reading-oriented publication** that needs to feel Apple-inspired. Defines hero, section rhythm, narrative pacing, scroll choreography, full-bleed media, and the three non-marketing archetypes (Tool / Workspace, Data / Dashboard, Editorial / Reading). Also use when the user complains "every section looks the same". Do NOT use for app-internal layout (use apple-ui-components) or for one-off widget composition.
version: "1.1.0"
license: MIT
---

# apple-web-composition

Where apple-design-foundations defines the **base**, this skill defines the **page**.
It is about what sections exist, in what order, with what rhythm — not the typography of any individual block.

## Hero — one focal point

A real Apple hero has **one dominant element** that the eye lands on first.

### Anatomy

- One large visual focal point: a hero product photo, a large type composition, a 360° product view, or a single full-bleed image. Pick **one**; not two competing focal points.
- One short headline (3–7 words, often the product or feature name).
- One supporting line (8–18 words), only if necessary.
- One primary CTA + one secondary CTA. Not three.
- Eyebrow (small uppercase tag) only if it adds navigational context (e.g. "MacBook Pro"). Don't use eyebrows as decoration.

### Patterns to avoid

- **Left headline + right dashboard card.** This is the SaaS default. Use only if the dashboard card is genuinely the hero subject.
- **Big stat trio** above the fold (3 giant numbers). This is the analytics-template default.
- **Headline + 3 feature cards stacked immediately under hero.** This collapses the hero into a section.

### First viewport rules

The first viewport (no scroll, ~700–900 px tall) must answer:

- What is this page about?
- What should I look at first?
- What can I do here?

Concretely:

- Headline scale ≥ 48 px on mobile, ≥ 72 px on desktop.
- Whitespace above the headline ≥ 12 vh.
- CTA prominence: one CTA above the fold; second CTA may be smaller / text-style.
- Media crop: the focal point must be optically centered, not pixel-centered.
- Visual balance: dark backgrounds balance with light text and vice versa — never all light.

## Section rhythm

Apple product pages follow a **musical** structure: not all sections sound the same.

### Pacing vocabulary

Pick **at least four** of these per long page. Repeating the same pattern kills the page.

- **Quiet hero** — large type, generous air, minimal decoration. Sets context.
- **Full-bleed image section** — one large image, short caption, no controls. Lets the product speak.
- **Pinned storytelling** — section sticks while content scrolls past; product rotates, disassembles, color-shifts. Used sparingly; one per page is enough.
- **Wide product gallery** — wide row of product photos or angles, scroll horizontally or grid.
- **Comparison / spec table** — restrained table with one or two highlighted columns.
- **Quote / proof point** — single sentence, generous air. Optional supporting attribution.
- **Feature detail** — large feature image + 2–4 sentences of body + a single visual that shows the feature.
- **Cross-sell row** — restrained horizontal scroller of related products.
- **Pricing** — one or three columns, no five-column comparison.
- **FAQ** — clean list, no accordion decoration.
- **Footer** — minimal, mostly legal + sitemap.

### Anti-pattern

**Every section is "title + 3 cards + next section".** This is the SaaS template default. Break it up.

A page with five consecutive "title + 3 cards" sections feels like a slide deck. Vary:

- Section height (tall / short / sticky / full-bleed).
- Section orientation (vertical / horizontal-scroll / asymmetric).
- Section density (one image / one quote / three cards / one table).
- Section background (white / off-white / image / dark).

### Transition rules

- Section padding should **vary** across the page. Identical padding everywhere feels mechanical.
- Backgrounds should **change** at least once per page in light mode (and several times in dark mode) — pure white everywhere is monotonous.
- Avoid hard color block transitions. Apple often uses a single off-white throughout, with image-led color shifts.

## Long pages — narrative arc

Treat the page as a story with quiet and loud passages:

```
[ Quiet hero ]
   ↓
[ Single full-bleed image or short feature intro ]
   ↓
[ Pinned storytelling or large feature 1 ]
   ↓
[ Quiet comparison or spec detail ]
   ↓
[ Full-bleed image section ]
   ↓
[ Pinned storytelling or large feature 2 ]
   ↓
[ Quiet FAQ / cross-sell / footer ]
```

At least one section should be **visually loud** (full-bleed, dark, large image) and at least one should be **visually quiet** (small text, generous air). Loud everywhere = exhausting. Quiet everywhere = boring.

## Product storytelling

When the page is about a product (or feature):

- Lead with the product or one image of it. Not with a UI dashboard.
- Show the product **doing the thing**, not just sitting on a backdrop.
- Use real photography or honest 3D renders. Stylized illustrations are fine for marketing but must be intentional.
- Captions are short. One sentence. If you need a paragraph, the image is doing too little work.
- Sticky / pinned sections are used **sparingly** — once per page max. They are expensive in attention.

## Scroll-driven narrative

- Reveal elements as they enter the viewport — but **vary the reveal**. Not every section fades up 20 px on scroll.
- Use scroll to drive **at most** one pinned sequence per page.
- Parallax is allowed **once**, on a hero image. Not on every section.
- Avoid scroll-jacking: the user's scroll should always feel under their control.

## Mobile composition

Mobile is not a shrunken desktop page.

- Hero stacks vertically: image first or text first, depending on the product.
- Large images become edge-to-edge (full-bleed).
- Cards stack. Cards in a 3-column row on mobile become a single column or a 2-up at most.
- Sticky elements (e.g. floating navbar) are simpler; avoid pinned storytelling on mobile.
- Section padding reduces by ~30–40% from desktop.
- Body text stays 16–17 px; never below 16 px.
- Tap targets ≥ 44×44 px.
- The fold (first 600–700 px) carries the headline + one CTA + maybe a small image. Nothing else.

## Non-marketing archetypes (added v1.1)

Apple-inspired composition applies to dense applications and data surfaces, not only to marketing pages. Two non-marketing archetypes:

### Tool / Workspace

For productivity tools, editors, design tools, content creation surfaces.

- **Hero:** the workspace itself. The "hero" is the first paint of the actual work surface, not a marketing panel.
- **Hierarchy:** clear primary action + secondary actions; deliberate grouping; tool affordances visible but quiet.
- **Density:** higher than marketing. Information density is a feature, not a bug. Whitespace is for legibility, not for "calm."
- **Navigation:** persistent left rail, top bar, or both. Search is a first-class control.
- **Chrome restraint:** less decoration around controls. The work is the product.
- **Disclose progressively:** advanced controls in inspector panels or menus, not in the main canvas.

### Data / Dashboard

For analytics, admin, monitoring, or any data-heavy surface.

- **Hero:** the first chart or key metric, not a headline + CTA.
- **Hierarchy:** primary metric, supporting metrics, drilldowns, filters. Numeric scale and grouping carry the hierarchy.
- **Density:** high. Dashboards ARE mostly cards — the difference between "card-everything" anti-pattern and a legitimate data dashboard is *content density per card*, not "fewer cards." If each card carries real data, it is structural, not decorative.
- **Navigation:** sidebar with category labels; tabs for sub-views; persistent search.
- **Filter chips:** pill-shaped chips are legitimate here. They are data controls, not decoration.
- **Radii adapt to density:** smaller radius (8–12 px) for cards; larger (12–16 px) for containers. Marketing-page radii (16–22 px) are too soft for data surfaces.
- **No glass on data surfaces.** Glass belongs to interaction overlays, not the data itself.

Composition **must not** force a marketing-page grammar onto application UI. A dense dashboard should not have an 80 px hero, a "storytelling" section arc, or a comparison table.

### Editorial / Reading

For long-form journalism, magazine layouts, essay-style publications, documentation sites, knowledge bases, and other content-first reading surfaces.

- **Hero = typography, not image.** A display headline, byline, and one supporting visual at most. The reading surface is the product, not the marketing message.
- **Hierarchy = reading flow.** Title -> dek -> body -> pull quote -> caption. Hierarchy is typographic scale and weight, not section dividers or callouts.
- **Density = generous.** Generous line-height (1.6-1.8 body), wide measure (~60-75 characters), single column or one narrow sidebar. Whitespace is the navigation.
- **Chrome restraint.** Persistent chrome (header, footer) is minimal. No competing interactive controls inside the article. Inline links, footnote markers, and one or two contextual CTAs at most.
- **Optional media pacing.** One hero image, occasional inline figures, optional pull quotes. No carousel, no autoplay, no popups.
- **Navigation that does not overwhelm content.** Table of contents (sticky or inline), back-to-top, related articles - present but quiet.
- **No forced sales CTA.** No "subscribe now" above the fold. No product placement. If monetization exists, it is contextual (post-article, sidebar) not forced.
- **No giant commercial hero.** No 80 px+ marketing headline. No alternating feature-section template.
- **No product-story layout.** Editorial is content-first; the article structure is the layout, not a sales funnel.
- **Liquid Glass on editorial surfaces = usually skip.** Editorial does not benefit from glass. Persistent chrome stays solid. Glass is reserved for floating interactive controls (e.g. share menu) if needed.

This archetype does not replace the 5 marketing hero patterns or the Tool / Workspace and Data / Dashboard archetypes. It is a third non-marketing path for content-first reading surfaces. Apply it when the user asks for a magazine, long-form article, journal, or documentation layout.

## Anti-patterns (composition-specific)

- "Left text + right dashboard mockup" hero on every page.
- Three-card features row repeated four times down the page.
- Five consecutive sections with the same vertical padding.
- A section with more controls (badges, pills, buttons) than content.
- A hero with three CTAs competing for attention.
- A footer with the same content as the navbar.

## Hard rules

- The first viewport answers *what is this page* without scrolling.
- The page has at least one visually loud section and at least one visually quiet section.
- Sections vary in height, density, and background — no mechanical repetition.
- Hero has one focal point. Not two.
- Mobile composition is a redesign, not a shrink.
- Scroll-driven sequences: at most one pinned section per page.
- Background changes at least once per page in light mode.

## Review checklist

Before declaring composition done:

- [ ] First viewport answers what / look-at / do without scrolling.
- [ ] Hero has one focal point (image or type composition), not two.
- [ ] Hero has 1–2 CTAs, not 3+.
- [ ] At least four different section types appear on long pages.
- [ ] No three-card-features repeated four times in a row.
- [ ] At least one full-bleed image section.
- [ ] At least one quiet (text-forward) section.
- [ ] Section backgrounds vary.
- [ ] Mobile is a redesign, not a shrink.
- [ ] At most one pinned scroll sequence per page.
- [ ] No scroll-jacking. Scroll always feels under user control.

## Common mistakes

- Stacking three "title + 3 cards" sections as a landing page.
- Headline + dashboard-card hero on every marketing page.
- Sticky storytelling on every section.
- Pure-white background everywhere.
- "Minimal" page with 80% empty viewport and no content scaffolding.
- Mobile page that is just the desktop layout scaled down.

## Companion references

- `references/hero-patterns.md` — hero anatomy examples and counter-examples.
- `references/section-vocabulary.md` — full library of section types.
- `references/scroll-storytelling.md` — pinned sequence patterns and pitfalls.
- `references/mobile-composition.md` — mobile-first composition rules.

This skill sits between foundations and components. Composition is the page; components are the parts.
