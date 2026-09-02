# CJK Typography Reference

Chinese (and more broadly CJK — Chinese / Japanese / Korean) typography for Apple-inspired web builds. Apple-inspired quality ≠ English-only SF-like typography.

This reference covers:

- Font fallback for CJK without infringing Apple font licensing.
- Display headings in Chinese.
- Mixed CJK + Latin (the common case in real Chinese products).
- Line length and rhythm.
- Responsive behavior.

The principle stays the same: hierarchy, restraint, optical balance, legibility. CJK is not a "translation" of Latin typography rules — it has its own optical reality.

## When to use

- A build has Chinese, Japanese, or Korean copy (titles, body, product names).
- A Chinese / Japanese / Korean product with mixed English tokens (e.g., an English product name inside Chinese paragraphs).
- A bilingual product launch page (e.g., 简体中文 + English).
- An international product where CJK is one of multiple scripts.

## Font fallback

SF Pro / SF Compact are Latin-only. They cover the ASCII / Latin glyphs but not CJK characters. On a Chinese system, the OS automatically substitutes the CJK glyphs from the system font — but only if your stack reaches it.

### Recommended CJK-first stack

```css
:root {
  --font-sans-cjk:
    "SF Pro", "SF Pro Display", "SF Pro Text",
    -apple-system, BlinkMacSystemFont,
    "Helvetica Neue",
    /* CJK fallbacks below — match the user's OS */
    "PingFang SC", "PingFang TC", "PingFang JP",  /* Apple platforms */
    "Hiragino Sans GB", "Hiragino Sans CNS",      /* Older Apple */
    "Microsoft YaHei", "Microsoft JhengHei",     /* Windows */
    "Source Han Sans SC", "Source Han Sans TC",  /* Adobe/Google, Linux */
    "Noto Sans SC", "Noto Sans TC", "Noto Sans JP", "Noto Sans KR",  /* Linux/Android */
    system-ui,
    "Segoe UI", Roboto, "Liberation Sans", sans-serif;
}
```

Notes:

- **Do not `@font-face` SF Pro** unless you have an explicit license from Apple. Apple has historically not licensed SF Pro for web redistribution.
- **Do not `@font-face` PingFang** — it ships with macOS / iOS only.
- For self-hosted CJK, use **Source Han Sans** (Adobe) or **Noto Sans CJK** (Google). Both are free for web embedding under SIL OFL.
- Chinese OS already has these; the stack above falls back automatically.

### Sans vs. serif for Chinese

- **Sans (Hei / Gothic):** modern, neutral, suits digital products. PingFang SC / Microsoft YaHei / Source Han Sans SC.
- **Serif (Song / Mincho):** editorial, traditional, suits publishing and luxury. Source Han Serif SC / 宋体 (SimSun) / Noto Serif CJK SC.

For Apple-inspired web product UI, **sans-serif** is the default. Reserve serif for editorial pages or specific brand moments.

## Display headings in Chinese

English display rules do not transfer to CJK. Before reading the values below, internalize the certainty level of each recommendation.

### Certainty levels

- **Hard.** CJK typography differs from Latin typography in measurable, repeatable ways. Do not skip these.
- **Heuristic.** A reasonable starting point for a common font and size. Verify visually for your specific font, size, and brand before committing. Tune as needed.
- **Context dependent.** The right value depends on font, script (Chinese / Japanese / Korean), brand, optical size, and surrounding composition. No single number is correct.

| Rule | Certainty | Source |
|---|---|---|
| Do not apply English negative tracking to CJK display | **Hard** | Stroke collisions and counter-shape loss are consistent across CJK fonts. |
| Visually verify any mixed CJK + Latin composition | **Hard** | Mixed scripts vary in metrics; the only verification is the rendered output. |
| English line-height rules are wrong for Chinese | **Hard** | Chinese characters' square bounding boxes make tight line-height visually cramped. |
| CJK display `font-weight: 500`, `line-height: 1.30` | **Heuristic** | Starting values for PingFang SC at 14–72 px. Verify for your font. |
| CJK body `line-height: 1.6–1.8` | **Heuristic** | Common range; tune for font and density. |
| CJK body `letter-spacing: 0.02em` | **Heuristic** | Slight positive tracking; depends on font. |
| CJK mixed-script `margin-inline: 0.15em` | **Heuristic** | Optical adjustment; skip when the font already provides natural spacing or the token is punctuation-bounded. |
| Inter-script spacing value (`margin-inline`) | **Context dependent** | Depends on font, punctuation, token (number / word / unit), and brand. |
| PingFang SC vs Noto Sans CJK vs Source Han Sans | **Context dependent** | Each font has its own optical metrics. |
| Chinese vs Japanese vs Korean | **Context dependent** | Each script has different rhythm and density. |

### Letter-spacing (hard + heuristic combined)

```css
/* Latin display: tight negative tracking */
.display-en {
  letter-spacing: -0.035em;
}

/* CJK display: NO negative tracking — characters look squished */
.display-cn {
  letter-spacing: 0;  /* or slightly positive, 0.02em for very large headings */
}
```

Negative tracking on Chinese display characters produces uneven density — strokes overlap and characters lose their counter shapes. Use `letter-spacing: 0` (or slightly positive) for Chinese display.

### Font weight (heuristic)

```css
/* Latin display */
.display-en { font-weight: 600; }

/* CJK display — 600 may render as 苹方粗体 which can feel heavy at large sizes */
.display-cn { font-weight: 500; }
```

For Chinese, **500** is usually enough for display. **700** is reserved for very short hero headlines where you want maximum impact. Verify against the actual rendered output for your font.

### Line-height (heuristic)

```css
/* Latin display: 1.05–1.10 */
.display-en { line-height: 1.08; }

/* CJK display: 1.20–1.40 — Chinese characters need more vertical air */
.display-cn { line-height: 1.30; }
```

Chinese characters have a uniform square bounding box and consistent stroke density; tight line-height makes them feel cramped. `1.30` is a typical comfortable Chinese display line-height. Verify for your font and size.

### Optical size (context dependent)

PingFang SC has separate "Display" and "Text" cuts. PingFang SC Display is optimized for ≥ 20 px; PingFang SC Text is for body. Browsers do not switch automatically — rely on size + stack. Other CJK fonts (Noto Sans CJK, Source Han Sans) have their own optical size rules; consult the font documentation.

### Character density (hard)

A Chinese display headline at 56 px has roughly 2× the visual density of an English headline at 56 px (each character occupies a fixed square). Two lines of 8-character Chinese = 16 glyphs vs 2 lines of 8-word English = ~16 glyphs but with much wider inter-word gaps. Don't try to make Chinese "as wide as English" — accept that Chinese display is more compact per line.

## Body text in Chinese

```css
.body-cn {
  font-size: 16px;  /* Apple keeps 17 px on phones; 16 px minimum */
  line-height: 1.7;  /* Chinese body benefits from 1.6–1.8 */
  letter-spacing: 0.02em;  /* slight positive for body */
  font-weight: 400;
}
```

Chinese body benefits from slightly looser line-height (1.6–1.8) than English body (1.45–1.5) because the characters are denser per line.

## Mixed CJK + Latin

The hard case: a Chinese paragraph with an English product name like **Helios 14** or a price **$449**.

### Stack selection

Use the same system stack with both Latin and CJK fallbacks. Browsers automatically pick the right font per character:

```css
:root {
  --font-mixed:
    "SF Pro", -apple-system, BlinkMacSystemFont,
    "Helvetica Neue",
    "PingFang SC", "PingFang TC",
    "Microsoft YaHei",
    "Noto Sans SC", "Noto Sans CJK SC",
    system-ui, sans-serif;
}
```

### Baseline alignment

Latin characters and CJK characters have different vertical metrics. When mixing inline, the baseline is usually correct (both share the same em baseline) but **optical center** may differ. Use `vertical-align` or `baseline-shift` for fine control:

```css
.mixed {
  font-feature-settings: "kern";
}

/* Center English caps inside CJK line */
.mixed .en {
  vertical-align: baseline;
  /* Avoid using sub/super to fake alignment */
}
```

### Weight mismatch

PingFang SC at weight 500 looks lighter than SF Pro at weight 500. When a CJK + Latin mix has a single font-weight, the Latin text dominates visually. Compensate:

- For CJK-heavy paragraphs, choose a heavier Latin weight (`-apple-system` at 500 looks "right" with PingFang 500).
- For Latin-heavy headers, let PingFang stay at 400 — Latin 600 wins visually.

### Number and English tokens

- Numbers in Chinese copy (e.g., 苹果公司 1976 年成立) are typically rendered in Latin numerals by default. For consistency, choose one:
  - Western Arabic numerals (`0123456789`) — standard for digital product UI.
  - Chinese full-width numerals (`０１２３４５６７８９`) — rare; reserved for formal publishing.
- English product names should use English typography rules, not CJK weight. Don't apply PingFang tracking to "Helios".

### Punctuation

Chinese full-width punctuation (`，。！？「」`) renders correctly when the font supports CJK. Don't try to substitute Latin punctuation — Chinese readers expect Chinese punctuation in Chinese text.

### Spacing between CJK and Latin

Browsers do **not** automatically add space between Chinese and adjacent Latin. The required spacing depends on **font metrics, punctuation, the specific Latin token (number vs. word vs. unit), brand typography, and context**. There is no universal value.

Treat the value below as a **heuristic starting point / optional optical adjustment**, not a rule. For fonts with naturally wide CJK glyphs (e.g. some Source Han Sans weights) or for tokens that already include their own breathing room (e.g. punctuation-spaced Latin), no extra margin may be needed at all.

```css
.mixed-cn-en .en {
  /* Heuristic only — tune for the actual font + content.
   * Skip when the font already provides natural spacing, or when the
   * Latin token is punctuation-bounded (e.g. "(macOS)"). */
  margin-inline: 0.15em;
}
```

**When NOT to apply this margin:**

- The CJK font already has wide glyphs with built-in side bearing.
- The Latin token is enclosed in its own punctuation (e.g. `(macOS)`) — the parens provide visual separation.
- The Latin token is a unit like `°C`, `18px`, `2024` — surrounding digits provide their own rhythm.
- The brand has a custom mixed-script style (e.g. Noto Sans CJK + Inter, with manual kerning) — defer to the brand's published rules.

Verify visually at the actual content size before committing.

## Line length

English measures line length in **characters** (35–75 chars per body line). For Chinese, the same rule applies but counts **CJK glyphs** (not bytes or words):

- Chinese body measure: 25–40 CJK characters per line.
- Chinese display headline: 8–14 CJK characters per line for impact.
- A 1080 px container at 17 px body text holds ~30 Chinese characters comfortably.

Don't apply `max-width: 65ch` blindly — `ch` is the width of "0" in the current font, which doesn't reflect CJK density. Use a fixed `max-width` (e.g., `720px`) and verify visually.

## Responsive behavior

```css
:root {
  --fs-display-xl: clamp(2.5rem, 8vw, 5.5rem);
  --fs-display-cn:  clamp(2rem, 6vw, 4.5rem);   /* Chinese display, slightly smaller */
  --fs-body:        clamp(1rem, 1vw + 0.875rem, 1.0625rem);
}
```

### Things to check at mobile width

- **Awkward 1-character line.** Chinese display headlines can end with a single character on the last line. This is the CJK equivalent of an orphan. Use `text-wrap: balance` (modern browsers) or manually break the headline.
- **Punctuation orphan.** Chinese opening quotes / brackets should not end a line; Chinese closing punctuation should not start a line. `text-wrap: balance` helps.
- **Overly tight line-height.** At small sizes, the Chinese body line-height should not drop below 1.6.
- **Tracking inverse.** Confirm Chinese display still has `letter-spacing: 0` at all viewport sizes.

### Mobile-specific

- Body never below 16 px.
- Don't let Chinese body shrink to 14 px — illegible.
- Display scaling: 56 → 40 → 32 px is reasonable for desktop → tablet → mobile.

## Common mistakes

- Applying `letter-spacing: -0.035em` to Chinese display — squishes characters.
- Setting Chinese body `line-height: 1.5` — too tight.
- `@font-face` SF Pro without a license — illegal for distribution.
- Using `ch` to measure Chinese line length — meaningless.
- Mixed CJK + Latin without explicit inter-script spacing.
- Weight mismatch: PingFang 500 + Latin 600 makes one side dominate.
- Letting Chinese body go below 16 px on mobile.

## Accessibility

- Maintain visual hierarchy through size, not just color.
- Chinese punctuation should be visible — check that full-width punctuation isn't clipped.
- Body line-height 1.6–1.8 supports reading for CJK users.
- `prefers-reduced-motion` rules apply regardless of script.

## What Apple actually does

Apple ships system fonts for CJK:
- **macOS / iOS:** PingFang SC (Simplified), PingFang TC (Traditional), PingFang JP (Japanese).
- **visionOS / Apple Vision Pro:** same family.
- Apple does NOT ship a web-embeddable CJK font from Apple Inc. for redistribution.

When you target Apple platforms, the OS substitutes PingFang automatically. When you target Windows / Linux / Android, you need to either self-host a CJK font or rely on system fonts.

## Recommended self-host fallback

If your build needs to look consistent across platforms:

```css
@font-face {
  font-family: "Brand Sans CJK";
  src: url("/fonts/SourceHanSansSC-Regular.otf") format("opentype");
  font-weight: 400;
  unicode-range: U+4E00-9FFF, U+3400-4DBF, U+3000-303F;  /* CJK ranges only */
  font-display: swap;
}
```

The `unicode-range` is critical — it tells the browser to load this font **only** for CJK characters, letting the system handle Latin and saving bandwidth.

Sources:

- Source Han Sans: https://github.com/adobe-fonts/source-han-sans (SIL OFL)
- Noto Sans CJK: https://github.com/notofonts/noto-cjk (SIL OFL)

Both are free for commercial use.