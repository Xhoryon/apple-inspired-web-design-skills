# Optical Fidelity Reference

The Optical Fidelity dimension of the Liquid Glass Fidelity Profile. This file is the Tier 2 reference for `apple-liquid-glass-fidelity`.

The goal of Optical Fidelity is **plausible local background response**, not "maximum distortion". The user should notice the interface before noticing the optical trick.

> **HARD RULE:** Apple-specific optical claims (chromatic aberration, depth-of-field, anisotropic distortion, caustics, lens dispersion, Fresnel simulation, physically accurate glass refraction) require Apple documentation specifically supporting the claim. Without that evidence, they are WEB OPTICAL APPROXIMATION HYPOTHESES (IMPLEMENTATION HEURISTIC / APPROXIMATION), never Apple canonical.

## What Optical Fidelity covers

A glass surface with high Optical Fidelity expresses:

- **Background-dependent displacement** (refraction approximation) — the surface's apparent position shifts based on the content behind it.
- **Local refraction** — at the edges, the content behind appears slightly bent or compressed.
- **Edge deformation** — the surface's edge is not a perfect straight line in heavy refraction.
- **Light / specular cues** — a moving highlight that follows the pointer or scrolls.
- **Blur / diffusion** — the backdrop is sampled with a soft falloff.
- **Luminosity response** — the surface tone shifts based on the backdrop's brightness.
- **Sampling region** — the surface reads the content directly behind it, not the whole page.
- **Restrained distortion** — the optical effect is present but never calls attention to itself.

A surface that achieves 4+ of these is optically expressive. One that achieves 1 or 0 is a flat pill or a static gradient.

## The hypothesis lab

The Web has several techniques for approximating optical behavior. Each is evaluated in the lab. The lab does not pretend that any single Web technique reproduces native Liquid Glass. The lab records the technique's **benefit, cost, artifacts, browser risk, accessibility risk, and verdict**.

### CSS `backdrop-filter`

| Field | Value |
|---|---|
| Benefit | Translucency, blur, saturate, contrast — the basic Web approximation. Wide browser support (with `-webkit-` prefix). |
| Cost | GPU; expensive on large surfaces; multiple stacked surfaces compound. |
| Artifacts | "Blur over white" if the backdrop has no optical information (see the stable v1.1 baseline). |
| Browser risk | Low; well-supported. |
| Accessibility risk | Low; `@media (prefers-reduced-transparency: reduce)` and `@supports not` are the standard fallbacks. |
| Verdict | **Baseline.** Required for any Web Liquid Glass. |

### CSS `backdrop-filter` with `saturate(180%)`

| Field | Value |
|---|---|
| Benefit | More vivid color infusion from the backdrop. Slight optical "lift". |
| Cost | Minimal additional cost over `blur` alone. |
| Artifacts | None if used modestly. |
| Browser risk | Low. |
| Accessibility risk | None. |
| Verdict | **Recommended.** Apple HIG mentions color infusion as a property of the material. |

### SVG `feDisplacementMap` (refraction approximation)

| Field | Value |
|---|---|
| Benefit | Real-feeling pixel-level displacement of the backdrop. Adds the visual cue of refraction. |
| Cost | GPU; significantly more expensive than `backdrop-filter` alone. |
| Artifacts | Visible if displacement scale is too high (>4 px). Moiré on certain patterns. |
| Browser risk | Medium. `feDisplacementMap` has historically been slow on Safari; check before shipping. |
| Accessibility risk | Low. Should be disabled in `prefers-reduced-transparency`. |
| Verdict | **Use sparingly.** Hero / primary surfaces only. ≤ 4 px scale. One surface per page at this level. |

### SVG `feTurbulence` (noise source)

| Field | Value |
|---|---|
| Benefit | Provides a non-uniform noise pattern for `feDisplacementMap` to consume. |
| Cost | Part of the displacement filter cost. |
| Artifacts | None if used inside the displacement filter; artifacts are the displacement's, not the turbulence's. |
| Browser risk | Medium (same as displacement map). |
| Accessibility risk | Same as displacement. |
| Verdict | **Component of the Level 2 displacement filter.** Not used alone. |

### CSS radial gradients (specular response)

| Field | Value |
|---|---|
| Benefit | Pointer-tracked highlight using CSS custom properties. Cheap, smooth. |
| Cost | Negligible if updated via `requestAnimationFrame` with idle-pause. |
| Artifacts | None if pointer-tracked and idle-paused. A static gradient is decoration, not specular response. |
| Browser risk | Low. |
| Accessibility risk | None (specular is independent of state). |
| Verdict | **Recommended for Level 2.** Required for any non-decoration specular response. |

### WebGL / WebGPU fragment shader

| Field | Value |
|---|---|
| Benefit | True optical approximation. Refraction, edge highlight, dynamic environment response, very subtle chromatic variation. |
| Cost | High. Must downsample (≤ 0.6x devicePixelRatio), pause when off-screen or idle, watch GPU. |
| Artifacts | Visible if shader is wrong (RGB fringe, band-of-color, over-shimmer). |
| Browser risk | High. WebGL performance varies wildly; WebGPU not yet broadly available. |
| Accessibility risk | High. Must respect `prefers-reduced-transparency` and `prefers-reduced-motion`. |
| Verdict | **Use only when the surface is the design (a hero centerpiece).** Never on more than one surface per page. Provide Level 1 fallback. |

### DOM / background duplication with sampling (offscreen canvas)

| Field | Value |
|---|---|
| Benefit | Real-time sampling of the area behind the surface, processed on a canvas. |
| Cost | Very high. Each frame reads the layout, samples pixels, processes. |
| Artifacts | Latency between pointer move and highlight update; tearing if not carefully rAF-throttled. |
| Browser risk | High. |
| Accessibility risk | High. |
| Verdict | **Rare.** Only when the visual effect is the centerpiece and no other technique is enough. |

## Chromatic aberration

| Field | Value |
|---|---|
| Status | **Web optical approximation hypothesis, not Apple behavior.** |
| Apple evidence | None as of 2026-09-03. |
| Why it is investigated | Real glass can disperse light. The hypothesis is that a tiny RGB shift at the edges adds a glass cue. |
| When to reject | If it produces RGB fringe, looks like gaming UI, harms text clarity, or calls attention to itself. |
| When to retain (rare) | As an extremely subtle edge shift, on a hero surface only, with a Level 1 fallback. Classified as `IMPLEMENTATION HEURISTIC / APPROXIMATION`. |
| Verdict | **Test, then likely reject.** Apple does not document chromatic aberration as a property of Liquid Glass. The Skill does NOT present it as Apple behavior. |

## Depth-of-field

| Field | Value |
|---|---|
| Status | **Web optical approximation hypothesis, not Apple behavior.** |
| Apple evidence | None as of 2026-09-03. |
| Why it is investigated | Real glass blurs distant content. The hypothesis is that a soft falloff of the backdrop adds depth. |
| When to reject | If the falloff looks like a camera lens blur, not material behavior. |
| When to retain (rare) | As a soft luminance falloff at the edges, on a hero surface only. Classified as `IMPLEMENTATION HEURISTIC / APPROXIMATION`. |
| Verdict | **Evaluate whether blur hierarchy alone achieves the design intent before adding DOF.** Apple does not document DOF as a property of Liquid Glass. |

## Anisotropic distortion

| Field | Value |
|---|---|
| Status | **Web optical approximation hypothesis, not Apple behavior.** |
| Apple evidence | None as of 2026-09-03. |
| Why it is investigated | Real glass with directional material geometry can refract differently in different directions. |
| When to reject | Always, unless the surface has actual directional geometry. |
| Verdict | **Reject unless geometry justifies it.** Anisotropic distortion for its own sake is rejected. |

## Caustics, lens dispersion, Fresnel simulation

| Field | Value |
|---|---|
| Status | **Web optical approximation hypothesis, not Apple behavior.** |
| Apple evidence | None as of 2026-09-03. |
| Verdict | **Reject for default Web approximation.** May be explored in the hypothesis lab as a curiosity; never shipped as "Liquid Glass" behavior. |

## Restraint invariant

> **The user should notice the interface before noticing the optical trick.**

If refraction becomes the visual subject, fail. A glass surface that requires the user to "see the glass" is wrong; the user is looking at content, not at the material. The material is in service of the content.

The candidate anti-pattern **Distortion Theater** (inherited from v1.1) applies: a glass surface that calls attention to its optical trick is wrong.

## Sampling region

Native `GlassEffectContainer` shares a sampling region across contained surfaces. The Web cannot reproduce this directly. The Web approximation:

- A coordinated glass cluster uses a single parent element whose children share the parent's `backdrop-filter` and optical effects.
- The implementer must verify visually that multiple nearby glass surfaces do not produce obviously inconsistent sampling.
- If inconsistency is visible, the design is broken; the implementer must either consolidate the surfaces into a single parent or accept the visual difference.

The Skill does NOT claim a Web container is equivalent to `GlassEffectContainer`. It records the Web analog as a coordinated cluster.

## Browser support matrix (snapshot 2026-09-03)

| Feature | Chromium | Safari | Firefox | Notes |
|---|---|---|---|---|
| `backdrop-filter` | Yes (no prefix) | Yes (`-webkit-` prefix required on older versions) | Yes (no prefix) | Widely supported. |
| `backdrop-filter: blur(>20px)` | Yes | Yes | Yes | Slower on large surfaces. |
| SVG `feDisplacementMap` | Yes | Historically slow on Safari; check | Yes | Test on Safari before shipping. |
| CSS `light-dark()` | Yes (Chromium 123+) | Yes (Safari 17.5+) | Yes (Firefox 120+) | Recent. |
| `prefers-reduced-transparency` | Yes | Yes | Yes (Firefox 113+) | Supported. |
| `Save-Data` | Yes | Yes | Yes | Save-Data API. |
| `navigator.deviceMemory` | Yes (Chromium) | No | No | Limited. |
| `navigator.hardwareConcurrency` | Yes | Yes | Yes | Supported. |
| WebGL 1 / 2 | Yes | Yes | Yes | Widely supported. |
| WebGPU | Limited (Chromium behind flag) | No (Safari announced) | Limited | Not yet broadly available. |

The implementer must verify on the target browser. The Skill records the principles; the implementer validates.

## What the Skill rejects

- Chromatic aberration as "Apple Liquid Glass has it" — UNSUPPORTED. May be tested as a Web hypothesis; rejected unless restrained.
- DOF as a default optical effect — UNSUPPORTED. Reject unless the design intent is hero-only.
- Anisotropic distortion for its own sake — REJECT.
- Caustics / Fresnel / dispersion as "Apple behavior" — UNSUPPORTED. Reject for default Web approximation.
- WebGL on every surface — REJECT (Shader Supremacy).
- Heavy `feDisplacementMap` (scale > 4 px) — REJECT.
- Stacked full-page glass layers — REJECT (glass-on-glass).
- A constant fill with no backdrop sampling — REJECT (not a material).

## Cross-Skill note

- The 7-dimension model (translucency, refraction, specular, adaptivity, depth, dynamics, morphing) is the stable v1.1 baseline. Optical Fidelity is the deepening of those dimensions.
- The stable `apple-liquid-glass-web` Skill's Level 0–3 ladder is the **Implementation Capability Level**, not the Optical Fidelity score.
- The motion values (spring stiffness, amplitude, duration) for any optical animation are HEURISTIC owned by `apple-motion-physics`.
- The legibility model is `glass-legibility.md`.
- The capability / fallback ladder is `web-capability-fallback.md`.
