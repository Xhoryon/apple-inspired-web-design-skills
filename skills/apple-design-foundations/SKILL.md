---
name: apple-design-foundations
description: Use when an Apple-inspired web build is in progress (typically first, after `apple-web-design` has confirmed Apple context), to establish typography, spacing, geometry, color, depth, and brand-override rules. Defines the foundational design language — content-first hierarchy, restrained typography, optical spacing, concentric geometry, restrained color, depth through material/motion not shadow. Apple-leaning values (off-white surfaces, near-black text, single restrained accent, modular spacing) are *starting heuristics* that yield to user brand tokens when present. Do NOT use for non-Apple designs, generic UI foundations, or as a general-purpose design encyclopedia. Do NOT trigger on any task that merely mentions typography or spacing — Apple context must already be established.
version: "1.1.0"
license: MIT
---

# apple-design-foundations

The **base layer** of the pack. Read this before any other skill. Every Apple-inspired decision downstream (composition, components, glass) inherits from these rules.

## 0. Brand override model

Apple-quality heuristics **yield** to existing brand tokens. The priority order is:

1. **Client / existing brand tokens** (color, type, voice, spacing tokens)
2. **Accessibility constraints** (contrast, motion, target size)
3. **Contextual design judgment** (density, intent, audience)
4. **Apple-inspired heuristics** (restraint, hierarchy, optical spacing, material behavior)

This means: a Spotify-green accent wins over an Apple-blue accent. A brand's custom serif wins over SF Pro. A dense dashboard layout wins over a marketing-page layout. **Apple-style restraint and quality still apply to the brand's palette and rules — not to Apple's.** The skill is a quality bar applied *within* the brand, not a brand replacement.

If the user has provided brand tokens (CSS variables, design system exports, or explicit values), honor them. Apple heuristics are the *default* — but always subordinate to brand.

### Palette classification

Values such as `#1D1D1F` (near-black), `#FBFBFD` (off-white), `#0066CC` (restrained blue), and neutral grays are **starting heuristics** for the Apple-leaning system palette, not a mandatory palette. They are:

- A default for projects without a defined palette.
- A reference for restraint (off-white vs. pure white, near-black vs. pure black, single accent vs. multi-hue).
- A starting point for typographic rhythm (font-weight scale, line-height range).

They are **not** a mandate to replace the user's brand colors. If the brand defines an accent color, use the brand color. If the brand defines typography, use the brand typography. Apply Apple-quality restraint to the brand's choices, not Apple's.

## 1. Content first

Interface serves content. Never the other way around.

- Every visual element must answer: *what content does this support, and what would the page look like without it?*
- If you can remove a card / pill / badge / icon / gradient / shadow / glass surface / decorative control and the page communicates better, **remove it**.
- The most Apple-like pages often have **fewer** visible UI affordances than a typical SaaS landing page.
- Hero is one message, not three. The product or one key image carries the page; supporting text is short.

**Red flag:** A section has more decoration than information. Strip decoration until information dominates.

## 2. Typography

Apple's web typography on its own pages feels *quiet* and *considered*. Replicate this through restraint, scale, and rhythm — not by hard-coding "use SF Pro".

### Font stack

SF Pro / SF Pro Display / SF Compact are Apple-licensed and not safely embeddable on every site. Use a system-first stack:

```css
--font-sans:
  "SF Pro Display", "SF Pro Text", "SF Pro",
  -apple-system, BlinkMacSystemFont,
  "Helvetica Neue", "Segoe UI",
  system-ui, "Segoe UI", Roboto, "Liberation Sans", sans-serif;
```

- Apple platforms pick up the SF branch automatically via `-apple-system`.
- Non-Apple platforms fall back to their own native UI font (Segoe UI on Windows, Roboto on Android, Noto on Linux).
- Never `@font-face` SF Pro unless you have an explicit license. Do not pretend Inter / Manrope / Geist is "Apple-like" by itself.

### Scale

Use a small number of clearly distinct sizes. Apple's pages typically rely on **two display sizes + one body + one caption**.

| Role | Size (desktop) | Weight | Line-height | Letter-spacing |
|---|---|---|---|---|
| Display XL (Hero headline) | 72–96 px | 600 | 1.05–1.08 | -0.02em to -0.035em |
| Display L (Section headline) | 48–64 px | 600 | 1.08–1.12 | -0.015em to -0.025em |
| Title | 28–36 px | 600 | 1.15–1.2 | -0.01em |
| Body lead | 21–24 px | 400 | 1.35–1.45 | 0 |
| Body | 17–19 px | 400 | 1.45–1.55 | 0 |
| Caption / eyebrow | 13–15 px | 500 | 1.3 | 0.02–0.06em (often uppercase) |

Notes:
- Tight tracking on large display, neutral on body, slight positive tracking on small uppercase eyebrows.
- Body **never** below 16 px on mobile. Apple keeps 17 px body even on phones.
- Line-height tightens as size grows. Do not use a single `1.5` for everything.
- Use a real type scale (modular, e.g. 1.25) — do not invent random sizes per section.

### Width

- Body measure: 60–75 characters (~640–720 px max-width at 17–19 px).
- Headlines can exceed this and feel comfortable.
- Avoid centered long-form prose.

### Hierarchy

Hierarchy is created by **size + weight + spacing**, not by color or by lower opacity. Do not compensate for missing size by greying out secondary text — give it a smaller size and a softer neutral.

## 3. Space

Apple's whitespace is **structural**, not decorative. It carries hierarchy and pacing.

- Use a **modular spacing scale** (4 or 8 px base). Apple uses 4 px and 8 px grid; pick one and stick to it.
- Section vertical rhythm: large displays get more air around them; small text gets less.
  - Hero padding-top: at least 12–16 vh of air above the headline.
  - Between sections: 80–160 px desktop, 56–96 px tablet, 48–72 px mobile.
  - Inside sections: 24–48 px between major blocks, 12–20 px between minor blocks.
- Horizontal gutters: clamp(20px, 4vw, 48px) for content; full-bleed media breaks out.
- Optical alignment beats mathematical alignment. A 24 px button next to a 28 px icon needs vertical nudging, not raw top alignment.
- Never fill space with cards or empty placeholder boxes. Empty space is a feature.

**Red flag:** A section is more than 50% empty background **and** the next section is immediately visible in the viewport. This is "giant empty space" — break up with content, imagery, or restructure.

## 4. Geometry

Apple's geometry is **concentric, optical, and varied by role**.

- **Concentric corners.** Nested surfaces reduce radius: container 28 px → inner card 20 px → button 980 px (pill). Do not stack identical radii.
- **Role-based radius.**
  - Window / sheet / large surface: 24–28 px.
  - Card / panel: 16–22 px.
  - Button: 8–14 px or pill, **not** always 24.
  - Tag / chip: pill or 6 px.
  - Icon button: 50% (circle).
- **Do not apply `border-radius: 24px` to everything.** A page where every element shares a radius looks plastic.
- **Edge-to-edge media** (full-bleed imagery, video) sits flush with the viewport. Don't round it.
- **Floating surfaces** (floating toolbars, sheets, popovers) are inset from the viewport edge by 12–24 px and have larger radius than inline surfaces.
- **Optical alignment:** text inside a button looks centered, but the optical center is ~1 px above mathematical center. Adjust line-height and padding to compensate.

## 5. Color

Apple's color is **restrained, semantic, and content-led**.

### Palette

- A single neutral axis (light mode: pure white → near-black greys; dark mode: near-black → off-white).
- **One** accent color that is semantic, not decorative. It marks a primary action or a status, never multiple unrelated elements.
- Most surfaces are neutral. Color usually comes from imagery (product photos, illustrations).
- Avoid: purple→blue gradients, neon glows, multi-hue gradients, accent on every interactive element.

### Light vs dark

Apple treats both as first-class. They are not afterthoughts.

- Light surfaces: warm off-white (`#FBFBFD` to `#F5F5F7` family), not pure `#FFFFFF` everywhere.
- Dark surfaces: deep neutral (`#000000` to `#1D1D1F`), not pure black text on pure black.
- Body text in light: `#1D1D1F` (not pure black).
- Secondary text: ~60% of primary contrast, not 30% (which is too grey to read).
- Borders: very subtle (`rgba(0,0,0,0.06–0.1)` in light, `rgba(255,255,255,0.08–0.12)` in dark).

### Contrast

- Body text on background must clear WCAG AA (4.5:1).
- Large display text needs 3:1.
- **Glass over imagery:** see apple-liquid-glass-web — text on translucent surfaces must either dodge busy areas or carry a contrast-protection layer.

## 6. Depth

Depth comes from **separation of concerns**, not stacked shadows.

Primary sources of depth, in order of importance:

1. **Content / control separation** — controls float, content reads. The user instantly knows what is interactive.
2. **Material** — glass, paper, video. See apple-liquid-glass-web.
3. **Overlap** — a floating toolbar overlapping an image creates more depth than a shadow.
4. **Scale** — size differences suggest foreground/background.
5. **Subtle shadow** — used sparingly. Apple uses **very subtle** shadows: ambient + key, low opacity, large blur, small offset.
6. **Motion** — material responding to scroll or pointer.

What depth is **not**:

- Five stacked `box-shadow` declarations.
- Heavy drop shadows with hard edges.
- Inset shadows on everything.
- Glow / blur halos around every element.

## Hard rules

- No element has all six depth sources. Pick one or two per element.
- A surface is either on the content layer (flat, opaque, body-color) or on the interaction layer (floating, often translucent, with material behavior). Do not mix.
- Never put body copy on a glass surface unless you have a verified contrast strategy.

## Review checklist

Use before declaring typography/space/color done:

- [ ] Body text is 17–19 px desktop / 16–17 px mobile, not smaller.
- [ ] Line-height varies by role (display tight, body 1.45+).
- [ ] Display headlines use negative letter-spacing; eyebrows use positive.
- [ ] No element uses `#000` text on `#FFF` background. Off-white / near-black is the rule.
- [ ] Spacing scale is consistent (one modular scale, no random 23/37 px values).
- [ ] Sections have rhythm: at least one section in the page has substantially different spacing than its neighbors.
- [ ] Nested corners are concentric (outer > inner).
- [ ] Page has **one** accent color, used semantically, not decoratively.
- [ ] Body text contrast passes WCAG AA.
- [ ] No five-layer shadow stacks on cards.

## Common mistakes

- Default browser typography (Times New Roman, blue links, etc.) shipping because no font stack was set.
- Using `border-radius: 24px` on everything.
- Pure white background everywhere.
- Greying out secondary text to 30% opacity — unreadable.
- "Minimal" page that is just giant whitespace and no content.
- Stacked shadows on a flat card "to add depth".

## Companion references

- `references/typography.md` — extended type recipes.
- `references/space-and-grid.md` — spacing scale and grid details.
- `references/geometry.md` — radius system and concentric corner rules.
- `references/color-and-depth.md` — palette construction and depth recipes.

This skill defines the **base**. Composition, motion, components, and glass all build on it.
