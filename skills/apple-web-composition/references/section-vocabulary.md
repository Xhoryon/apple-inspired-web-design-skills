# Section Vocabulary Reference

Extended recipes for Apple-inspired section types. Builds on `apple-web-composition/SKILL.md`.

The point of having a section vocabulary is to **vary** section types so the page reads as music, not a slide deck.

## Section types — pick at least four per long page

### 1. Quiet hero

Big type, generous air, minimal decoration. Sets context.

```
[ big headline ]
[ short supporting line ]
[ 1-2 CTAs ]
```

### 2. Full-bleed image section

One large image, short caption, no controls. Lets the product speak.

```
[ edge-to-edge image ]
[ short caption, optional ]
```

### 3. Pinned storytelling

Section sticks while content scrolls past. Product rotates, disassembles, color-shifts.

```
[ pinned stage, scroll-driven sequence inside ]
```

Use sparingly. Once per page.

### 4. Wide product gallery

Wide row of product photos or angles. Horizontal scroll or grid.

```
[ image ] [ image ] [ image ] [ image ] →
```

### 5. Comparison / spec table

Restrained table with one or two highlighted columns.

```
| Feature | A     | B     | C     |
|---------|-------|-------|-------|
| ...     | ...   | ...   | ...   |
```

### 6. Quote / proof point

Single sentence, generous air. Optional attribution.

```
[ large quote ]
— attribution
```

### 7. Feature detail

Large feature image + 2–4 sentences of body + a single visual.

```
[ image or render ]    Headline
                       Short body
                       Optional CTA
```

### 8. Cross-sell row

Restrained horizontal scroller of related products.

```
[ product ] [ product ] [ product ] [ product ] →
```

### 9. Pricing

One or three columns, not five.

```
[ plan ]    [ plan ]    [ plan ]
```

### 10. FAQ

Clean list, no accordion decoration.

```
Q: ...
A: ...

Q: ...
A: ...
```

### 11. Footer

Minimal: legal, sitemap, region selector.

```
[ logo ]   [ sitemap columns ]   [ region / language ]
```

## Patterns to avoid

### Title + 3 cards

The single most overused pattern. **Don't repeat it more than twice on a long page.**

```
[ Section title ]
[ short supporting line ]
[ card ] [ card ] [ card ]
```

### Title + 4 cards

Same problem, worse.

### Bento grid section

Once or twice per page is fine. Three or more is "bento everywhere".

### Centered text + centered image (every section)

Centered everything is monotonous. Vary alignment.

### Identical padding (every section)

Same vertical padding everywhere is mechanical. Vary.

### Same background color (every section)

Pure white everywhere is monotonous. Vary.

## Section transition patterns

### Hard cut

Section A ends, Section B begins. Use sparingly.

### Background shift

Section A is white, Section B is tinted, Section C is image, Section D is dark. The shift carries rhythm.

### Spacer

A short section with just a quote or single line of large text. Use to break up dense sections.

### Vertical rhythm

| Section | Padding-top | Padding-bottom | Background |
|---------|--------------|----------------|------------|
| Hero    | 12–16 vh     | 8–12 vh        | neutral   |
| A       | 8 vh         | 8 vh           | neutral   |
| B       | 12 vh        | 12 vh          | tinted    |
| C       | 0            | 0              | image     |
| D       | 12 vh        | 12 vh          | dark      |
| E       | 8 vh         | 16 vh          | neutral   |

Note the variation. The image section (C) breaks the rhythm by going to 0 padding (full-bleed).

## Anti-patterns

- 5 consecutive "title + 3 cards" sections.
- All sections with same padding.
- All sections same background.
- Section with more decoration than content.
- Section that is mostly empty space.

## Accessibility

- Maintain heading hierarchy (h1 → h2 → h3) across sections.
- Don't skip heading levels.
- Each section has a clear semantic role (`<section>`, `<article>`, `<aside>`).
- Use `aria-labelledby` to point sections at their headings.
- Avoid section backgrounds that are too close to body text contrast.
