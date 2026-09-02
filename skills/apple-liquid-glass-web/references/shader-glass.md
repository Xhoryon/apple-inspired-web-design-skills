# Shader Glass — Level 3 Recipes

Level 3 implements Liquid Glass with a WebGL or WebGPU fragment shader. Real refraction, real specular response, real edge deformation. Use only when the surface **is** the design (a single hero element) or when the user has explicitly requested a shader-quality effect.

## When to use

- A hero centerpiece floating control that defines the page.
- A premium marketing effect where the surface carries the brand.
- Cases where Level 1 + Level 2 don't deliver enough optical realism.

Do **not** use:

- On every navbar, every toolbar, every sheet.
- On more than one surface per page.
- On low-end devices.
- When the page has other expensive rendering (large images, video, other shaders).

## The basic architecture

```
┌──────────────────────────────┐
│  Canvas (the surface)        │
│                              │
│  Shader samples a            │
│  texture (the backdrop)      │
│  and applies:                │
│   - edge displacement         │
│   - refraction               │
│   - specular highlight       │
│   - adaptive tint            │
│                              │
└──────────────────────────────┘
```

The backdrop is captured from behind the surface, then warped and tinted by the shader.

## A minimal WebGL fragment shader

```glsl
precision highp float;

uniform sampler2D uBackdrop;
uniform vec2 uResolution;
uniform vec2 uPointer;
uniform float uTime;
uniform float uPointerIntensity;

varying vec2 vUv;

void main() {
  vec2 uv = vUv;
  vec2 fromCenter = uv - 0.5;
  float dist = length(fromCenter);

  // Edge-driven refraction — light bends more at edges
  vec2 refractedUv = uv + normalize(fromCenter) * pow(dist, 2.0) * 0.03;
  vec4 bg = texture2D(uBackdrop, refractedUv);

  // Specular highlight near pointer
  float spec = exp(-pow(distance(uv, uPointer) * 30.0, 2.0)) * uPointerIntensity;

  // Edge tint — top catches light
  float edgeLight = smoothstep(0.0, 0.15, uv.y) * (1.0 - smoothstep(0.15, 0.4, uv.y));
  vec3 tint = vec3(1.0);
  tint += vec3(0.05) * edgeLight;

  // Combine
  vec3 color = bg.rgb * tint + spec * 0.18;
  float alpha = 0.78 + edgeLight * 0.15;

  gl_FragColor = vec4(color, alpha);
}
```

Notes:

- `pow(dist, 2.0) * 0.03` controls refraction strength. 0.03 is subtle; 0.10 is heavy.
- `exp(-pow(...)*30.0)` creates a soft specular hotspot. Higher exponent = tighter.
- `uPointerIntensity` ramps 0→1 when pointer moves, ramps back to 0 when idle.

## Vertex shader

```glsl
attribute vec2 aPosition;
varying vec2 vUv;

void main() {
  vUv = aPosition * 0.5 + 0.5;
  gl_Position = vec4(aPosition, 0.0, 1.0);
}
```

## Wiring in JS (vanilla WebGL)

```js
const canvas = document.querySelector("#glass-canvas");
const gl = canvas.getContext("webgl", { premultipliedAlpha: true });

// Vertex buffer: a fullscreen quad
const quad = new Float32Array([-1, -1, 1, -1, -1, 1, 1, 1]);
const buffer = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, buffer);
gl.bufferData(gl.ARRAY_BUFFER, quad, gl.STATIC_DRAW);

// Compile and link shaders
const vs = gl.createShader(gl.VERTEX_SHADER);
gl.shaderSource(vs, vertexShaderSource);
gl.compileShader(vs);

const fs = gl.createShader(gl.FRAGMENT_SHADER);
gl.shaderSource(fs, fragmentShaderSource);
gl.compileShader(fs);

const program = gl.createProgram();
gl.attachShader(program, vs);
gl.attachShader(program, fs);
gl.linkProgram(program);
gl.useProgram(program);

const aPos = gl.getAttribLocation(program, "aPosition");
gl.enableVertexAttribArray(aPos);
gl.vertexAttribPointer(aPos, 2, gl.FLOAT, false, 0, 0);

const uBackdrop = gl.getUniformLocation(program, "uBackdrop");
const uPointer = gl.getUniformLocation(program, "uPointer");
const uPointerIntensity = gl.getUniformLocation(program, "uPointerIntensity");
const uResolution = gl.getUniformLocation(program, "uResolution");

// Create backdrop texture from a hidden canvas that mirrors the page behind
const backdrop = document.querySelector("#glass-backdrop-capture");
const tex = gl.createTexture();
gl.bindTexture(gl.TEXTURE_2D, tex);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_S, gl.CLAMP_TO_EDGE);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_WRAP_T, gl.CLAMP_TO_EDGE);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);

// Render loop
let pointer = [0.5, 0.5];
let pointerIntensity = 0;
let lastMove = 0;
let raf = 0;
let isVisible = true;

const render = () => {
  if (!isVisible) {
    raf = requestAnimationFrame(render);
    return;
  }
  // Update backdrop texture from capture canvas
  gl.bindTexture(gl.TEXTURE_2D, tex);
  gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, backdrop);

  // Decay pointer intensity
  pointerIntensity *= 0.92;

  gl.uniform2f(uPointer, pointer[0], pointer[1]);
  gl.uniform1f(uPointerIntensity, pointerIntensity);
  gl.uniform2f(uResolution, canvas.width, canvas.height);
  gl.uniform1i(uBackdrop, 0);

  gl.drawArrays(gl.TRIANGLE_STRIP, 0, 4);

  raf = requestAnimationFrame(render);
};

// Pointer tracking
canvas.addEventListener("pointermove", (e) => {
  const rect = canvas.getBoundingClientRect();
  pointer[0] = (e.clientX - rect.left) / rect.width;
  pointer[1] = 1 - (e.clientY - rect.top) / rect.height;  // flip Y for GL
  pointerIntensity = 1;
});

document.addEventListener("visibilitychange", () => {
  isVisible = !document.hidden;
});

render();
```

## Capturing the backdrop

The shader needs pixels from behind the surface. Three approaches:

### A. Mirror via DOM (limited)

Position a duplicate of the page content absolutely behind the surface. Cheap but only works for static content.

### B. html2canvas snapshot

Capture the page (or a region) and feed the canvas to the shader. Heavy on mount; ok for hero.

```js
import html2canvas from "html2canvas";

const snap = await html2canvas(document.body, { backgroundColor: null });
gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, snap);
```

### C. CSS `backdrop-filter` + canvas screenshot (hybrid)

Use the browser's compositor (`backdrop-filter`) to render the backdrop, then read it into a canvas via `html2canvas` or by drawing the blurred region. Complex; rarely needed.

## Performance budget (Level 3)

- **One** shader-glass surface per page. Two is too many.
- Downsample the framebuffer: `canvas.width = displayWidth * 0.6`.
- Cap pointer update rate: rAF throttled, only update texture when pointer moves.
- Pause on `visibilitychange`, on battery saver, on `Save-Data`, on `navigator.hardwareConcurrency < 4`.
- Use `powerPreference: "low-power"` when getting the WebGL context on low-end devices.
- Provide Level 1 fallback when WebGL is unavailable, when `prefers-reduced-transparency`, when the device is detected as low-end.

## Pause / dispose

```js
const stop = () => {
  if (raf) cancelAnimationFrame(raf);
  raf = 0;
  gl.deleteTexture(tex);
  gl.deleteBuffer(buffer);
  gl.deleteProgram(program);
};

const start = () => {
  if (!raf) render();
};

// Pause when off-screen
const io = new IntersectionObserver((entries) => {
  entries.forEach((e) => (e.isIntersecting ? start() : stop()));
});
io.observe(canvas);
```

## Accessibility

- `prefers-reduced-transparency: reduce` → Level 1.
- `prefers-reduced-motion: reduce` → no animated specular; static highlight or none.
- Always pair with a non-shader fallback (Level 1) for browsers without WebGL.
- Provide a static version of the surface for screen readers (semantic HTML inside the canvas).
- Don't trap focus inside the canvas.

## What shader glass is NOT

It is not a license to ignore the rest of the design system. A shader-glass hero is impressive for one moment. If the rest of the page is a default browser page with `backdrop-filter: blur(12px)`, the contrast makes the shader look gimmicky. Use Level 3 only when the rest of the page is at Level 1+ in restraint and craft.
