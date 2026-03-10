# Stress Test — Voiceover-Driven Generative 3D Data Narrative (Mobile-First)

> Date: 2026-03-10
> This is not a brief. This is a capability evaluation disguised as a brief. Your first task is to dismantle it.

---

## Preamble — Read This Before You Write a Single Line

**Do not start building yet.**

Before you touch code, you must complete the following meta-tasks. These are not optional. They are the actual test.

### Phase 0: Research (mandatory)

You are operating in a field that changed materially in the last 14 days. Your training data is stale. Before making any architectural decisions, you must:

1. **Search the web** for the current stable version of Three.js (hint: it is NOT r170). Read the changelog. Identify breaking changes since r170 that affect this project.
2. **Search the web** for WebGPU browser support as of today. Determine whether `WebGPURenderer` with automatic WebGL 2 fallback is viable for a mobile-first audience. Cite your sources.
3. **Search the web** for TSL (Three.js Shading Language). Determine whether raw GLSL vertex/fragment shaders are still the correct approach or whether TSL node materials now offer equivalent power with better cross-backend compatibility.
4. **Search the web** for the latest post-processing architecture in Three.js. The class names have changed. `EffectComposer` may be deprecated. Find out.
5. **Search the web** for CSS `animation-timeline: scroll()` and `animation-timeline: view()` browser support. Determine whether native scroll-driven CSS animations are now cross-browser baseline and whether they could replace JavaScript scroll-binding for any part of this project.
6. **Search the web** for GSAP's current version and licensing status (it changed recently).
7. **Search the web** for ElevenLabs' browser/client SDK. Determine the correct npm package name, streaming API shape, and whether it can run from a single HTML file via CDN.

**Output a research summary** at the top of your response before any code. Include version numbers, browser support tables, and URLs. If you skip this step, the entire output is invalid.

### Phase 1: Brief Audit (mandatory)

Read the full brief below. Then write a section titled **"Brief Audit"** where you:

1. **Identify at least 5 holes, contradictions, or underspecified areas** in this brief. Be specific. Examples: timing assumptions that don't account for variable speech synthesis latency, visual mode definitions that conflict with mobile GPU budgets, missing accessibility considerations, architectural decisions that are already outdated.
2. **Identify at least 3 things this brief asks for that are wrong or suboptimal** given March 2026 technology. Challenge the brief. If the brief says "use X" but Y is now demonstrably better, say so and explain why.
3. **Propose your own amendments** to the brief. These become your actual specification. You are not bound by the brief — you are bound by the goal of producing the best possible output.

If your Brief Audit is shallow ("the brief is mostly good, here are some minor notes"), you have failed the test. The brief is intentionally flawed. Find the flaws.

### Phase 2: Build

Only after completing Phase 0 and Phase 1 should you write code. Your code must reflect the research and audit — not the original brief where the brief was wrong.

---

## What You Are Building

A single-file, mobile-first (9:16), voiceover-driven generative 3D experience. It plays a ~70-second narrated script about a brand competition — a human-crafted brand versus an AI-generated brand — across Google, Meta, and Email channels. The narration drives every visual event. There is no scroll. There is no user interaction beyond tapping to start.

The visual centrepiece is a procedural organic form — a living, breathing 3D entity that fills the viewport and reacts to the meaning of every spoken line. It is not decoration. It is not a background. It IS the content. The words appear ON it, THROUGH it, AROUND it. The viewer should feel like they are inside the organism looking out, not outside looking in.

This must feel like a film, not a slideshow. Zero dead air between lines. Visual transitions must be aggressive, immediate, and semantically meaningful. When the script says "fire", the form erupts. When it says "split", the form tears apart. When it says "locked", a physical lock manifests. Every word drives geometry.

The target audience views this on a phone, in portrait, probably on Instagram or TikTok. If it doesn't hit hard on a 390×844 viewport at 60fps, it doesn't exist.

---

## The Script

Each cue has: start time (`t`), duration (`d`), spoken text (`say`), display text (`html`), visual mode (`m`), speech rate (`r`), and pitch (`p`). The visual mode is a key that maps to a complete visual state change — geometry, colour, camera, lighting, particles, post-processing. Every mode must be a distinct world.

```
t      d    say                                                html                                                                      m          r     p
0      3.5  Welcome to Day SIX of our journey.                 Welcome to Day <b class="f">SIX</b>                                       intro      0.82  0.70
3.8    2.8  ONE brand, crafted by human hands.                 <b class="h">ONE</b> brand                                                human      0.92  0.85
7.0    2.5  ONE, generated by A.I.                             <b class="a">ONE</b> generated by A.I.                                    ai         0.92  0.85
10.0   2.8  Same constraints. Same timeline.                   Same constraints. Same timeline.                                           split      1.00  0.95
13.2   3.5  We are watching, who survives the fire.            Who survives the <b class="f">fire</b>.                                   fire       0.78  0.65
17.0   2.2  To win, you need to PERFORM.                       <b class="g">PERFORM</b>.                                                 perform    1.12  0.95
19.5   3.8  Three channels. Google. Meta. Email.               <span class="a">Google.</span> <span class="a">Meta.</span> <span class="a">Email.</span>  channels   1.10  0.95
23.8   3.0  Same creative volume. Same spend. Same conditions. Same volume. Same spend.<br/>Same conditions.                              equal      1.02  0.95
27.2   3.2  Both brands go head-to-head, across ALL THREE.     <span class="f">Head-to-head</span> across <b>ALL THREE</b>.              battle     0.85  0.75
30.8   2.5  Category ONE is performance.                       Category <b class="g">ONE</b>                                             cat1       0.92  0.85
33.8   3.0  HARD DATA. Clicks, conversions, results.           <b class="mono">HARD DATA.</b>                                            data       0.92  0.85
37.2   2.8  The stuff that ACTUALLY pays the bills.            <span class="g">ACTUALLY</span> pays the bills.                           data2      0.88  0.82
40.3   2.0  No opinions. Just numbers.                         <b class="a mono">Just numbers.</b>                                       numbers    1.05  0.95
42.8   2.0  Category TWO?                                      <i>Category TWO?</i>                                                      mystery    0.70  0.50
45.2   3.2  We're keeping THAT one, locked, for now.           <span class="f">Locked</span>... for now.                                 locked     0.92  0.85
48.8   3.0  It will reveal itself, when the time is right.     When the time is right.                                                    reveal     0.78  0.65
52.2   2.8  WE handle the operational data gathering.          <b>WE</b> handle the data.                                                ops        0.92  0.85
55.5   2.5  But YOU, set the benchmark.                        <b class="g">YOU</b> set the benchmark.                                   you        0.90  0.85
58.5   3.2  What METRICS prove a brand is actually working?    What <span class="a">METRICS</span> prove it?                             question   0.88  0.82
62.0   2.2  Comment your thoughts below.                       Your thoughts.                                                             cta        0.92  0.85
64.5   3.5  See you again, soon.                               See you <span class="a">soon</span>.                                     outro      0.85  0.80
```

Total runtime: ~69 seconds. Gaps between cues must be near-zero. The voice should still be finishing one line as the visual for the next begins transitioning. Overlap is preferable to silence.

---

## Mobile-First Viewport (9:16)

This is designed for vertical phone screens. Every design decision flows from this:

- **Viewport:** 390×844 logical pixels (iPhone 14/15 standard). Test at this size. If it also works at 360×800 (Android) and 428×926 (iPhone Pro Max), excellent. Desktop is a bonus, not a target.
- **The organism fills the vertical frame.** Not centred in a square. Not floating in space. It occupies 70–90% of the screen height. The camera is close. Claustrophobically close.
- **Text is overlaid directly on the 3D canvas.** Large, bold, readable against the moving form. Use `mix-blend-mode: difference` or similar compositing. Text should feel burned into the surface.
- **Touch: tap to start, nothing else.** No scroll, no drag, no pinch. This is a lean-back viewing experience.
- **Aspect ratio lock:** If the viewport is wider than 9:16 (desktop, landscape phone), letterbox with black bars or constrain the canvas to a centred 9:16 column. The composition must not break.

---

## The Organism

The central form is a high-detail procedural mesh. It must feel alive — not like a static sphere with noise displacement, but like a creature breathing, thinking, reacting. Requirements:

- **Geometry:** Start with a sphere or icosahedron, but the displacement must be extreme enough that the base shape is unrecognisable. The viewer should not think "that's a sphere with noise on it." They should think "what IS that thing?"
- **Vertex displacement:** Multi-octave noise (FBM or similar), but layered with mode-specific deformations. Each visual mode must produce a geometrically distinct silhouette. "Fire" should have upward tendrils and violent spikes. "Split" should physically tear the mesh into two hemispheres. "Data" should impose geometric grid patterns onto the organic surface. "Mystery" should collapse into something small and unknowable.
- **Material:** Not MeshStandardMaterial. A custom shader with:
  - Fresnel rim lighting (strong — this is what reads on mobile)
  - Subsurface scattering approximation (light bleeding through thin edges)
  - Colour split: human side (hot pink #ff1a5e) vs AI side (cyan #00d4ff), blending through the centre
  - Fire mode: incandescent orange/yellow emission from the surface
  - Data mode: circuit-pattern grid overlay (procedural, animated, scrolling)
  - Emissive core glow that pulses with speech rhythm
- **Scale:** The organism must be LARGE in frame. Camera distance should range from z=10 (intimate, organism fills everything) to z=24 (pulled back for context), but the default is close. The viewer's instinct should be to lean back from their phone.

---

## Particle System

8,000+ particles orbiting the organism. These are not decorative dust — they are the organism's atmosphere. They respond to every mode change:

- **Orbit:** Default state. Gentle rotation around the form.
- **Swarm/separate:** During "human" and "ai" modes, particles split into two clouds — pink left, cyan right.
- **Eruption:** During "fire", particles go chaotic — violent displacement, upward trajectories, increased brightness.
- **Data streams:** During "data" modes, particles reorganise into vertical columns — falling data rain.
- **Convergence:** During "you" and "perform", particles rush inward toward the form — compression, intensity.

Particles must be rendered with additive blending, soft edges (distance-based alpha), and size variation. On mobile, consider reducing count to 4,000–6,000 and compensating with larger particle sizes.

---

## Camera

The camera is always looking at the origin. It moves continuously — never static. Movement is subtle orbital drift during calm moments, aggressive repositioning during transitions.

- **Default:** Slightly offset from centre (not dead-on z-axis), gentle sinusoidal orbit.
- **Fire/battle:** Camera shake. Use damped noise, not random jitter. The shake should feel like the camera operator flinched, not like the feed is buffering.
- **Mystery/locked:** Camera pulls back slowly. Creates negative space.
- **You/perform:** Camera pushes IN. The organism is inches away. Almost uncomfortably close.
- **All transitions:** Lerp-based, speed ~0.06. Snappy, not floaty. The camera should arrive at its destination before the viewer consciously notices it moved.

---

## Lighting

Not basic. Multiple light sources that change with every mode:

- **Ambient:** Low (0.3–0.4 intensity). The scene should be predominantly lit by the organism itself and accent lights.
- **Directional:** Subtle fill from upper-right. Always on, low intensity.
- **Point lights (×3 minimum):**
  - Human accent (hot pink) — activates during human-related cues, positioned left
  - AI accent (cyan) — activates during AI-related cues, positioned right
  - Fire accent (orange-red) — activates during fire/battle cues, positioned below-centre
- All point light intensities lerp to their targets. No snapping.

---

## Post-Processing

Film-grade pipeline. Every pass must justify its GPU cost on mobile:

- **Bloom:** Selective. Only emissive surfaces and bright specular highlights glow. Use `UnrealBloomPass` or equivalent. Threshold tuned so the dark background doesn't bloom.
- **Chromatic aberration:** Radial, from screen centre. Intensity scales up during fire/battle/perform modes, near-zero during calm modes.
- **Vignette:** Constant, moderate. Dark edges with a slight colour tint (deep purple or deep blue). Frames the organism.
- **Output/tone mapping:** ACES Filmic. Exposure ~1.1.

On low-end mobile GPUs, the chromatic aberration pass should be the first to drop. Bloom and vignette are non-negotiable.

---

## Typography

- **Primary font:** Inter (variable weight, 100–900) via Google Fonts.
- **Monospace accent:** JetBrains Mono for data-related text.
- **Display text:** Large. `clamp(2.4rem, 7vw, 6.5rem)` or similar. Weight 100–200 for elegance, 900 for emphasis words.
- **Colour coding via CSS classes:**
  - `.h` — human colour (#ff1a5e)
  - `.a` — AI colour (#00d4ff)
  - `.f` — fire colour (#ff3300)
  - `.g` — gold/performance colour (#ffbb00)
  - `.b` — bold (weight 900)
  - `.i` — italic, reduced opacity
  - `.mono` — JetBrains Mono
- **Transitions:** Text fades in with a slight scale-up (0.92 → 1.0) over 250ms. Fades out 400ms before the next cue fires. No overlap of text — one line at a time.
- **Punch animation:** On high-energy cues (fire, perform, you), text scales up to 1.15 then snaps back to 1.0 over 300ms. No transition — CSS animation.

---

## Voice

**Placeholder:** Web Speech API (`SpeechSynthesisUtterance`). Select a male English voice. Rate and pitch per cue are specified in the script table.

**Production intent:** ElevenLabs will replace Web Speech API. Design the voice system as a swappable module — a single `say(text, rate, pitch)` function that the rest of the codebase calls. When ElevenLabs is integrated, only this function changes.

Speech synthesis `onend` events should NOT drive cue timing. Cues are driven by a master clock (`performance.now()`). Speech is fire-and-forget. If the voice runs long, the next cue's visual still fires on time. The voiceover catching up is acceptable; the visuals lagging is not.

---

## HUD Elements

- **Channel bars:** Three small bar chart groups (Google, Meta, Email), each showing human vs AI performance as thin horizontal bars. Appear during data-related cues. Position: bottom-centre of viewport.
- **Progress bar:** 2px tall, full viewport width, at the very bottom. Gradient from human colour to AI colour. Width = elapsed time / total duration.
- **CTA:** "Comment your thoughts" appears during the `cta` cue. Simple text + outlined button. Appears centre-bottom.
- **VS flash:** During the `perform` cue, a massive "VS" fills the screen momentarily (opacity 0 → 1 → 0 over ~1.5s). Enormous font size. Gradient from human to fire colour. `drop-shadow` glow.

---

## Architecture

Single HTML file. No build step. CDN imports only.

```html
<script type="importmap">
{
  "imports": {
    "three": "https://cdn.jsdelivr.net/npm/three@<LATEST>/build/three.module.js",
    "three/addons/": "https://cdn.jsdelivr.net/npm/three@<LATEST>/examples/jsm/"
  }
}
</script>
```

**You must determine `<LATEST>` from your research.** Do not hardcode r170.

The entire codebase lives in a single `<script type="module">` block. Structure:

1. **Renderer setup** (WebGPU with WebGL 2 fallback, OR WebGL if WebGPU is not yet viable — justify your choice)
2. **Scene, camera, lights**
3. **Organism** (geometry + custom shader material)
4. **Particle system** (buffer geometry + shader material)
5. **Supplementary objects** (lock, any other mode-specific geometry)
6. **Post-processing pipeline**
7. **Voice module** (swappable `say()` function)
8. **Script definition** (cue array)
9. **Visual state machine** (mode handlers that set target values)
10. **Animation loop** (lerps all values toward targets, updates uniforms, renders)
11. **Resize handler** (debounced, maintains 9:16 framing)
12. **Error boundary** (try/catch around entire module, renders error to DOM on failure)

---

## What "Better" Means

You are expected to exceed this specification. Directions to consider:

- **Geometry morphing between cues** — not just displacement changes, but topological shifts (sphere → torus → shattered fragments → reconvergence)
- **Instanced secondary geometry** — floating data shards, glowing runes, wireframe echoes of the main form
- **Audio-reactive scaling** — if Web Speech API exposes audio data, use it to drive uniform values in real-time
- **View-dependent effects** — parallax layers behind the organism, depth-aware blur on non-focused elements
- **Gyroscope input** — on mobile, subtle camera offset based on device orientation (with `DeviceOrientationEvent` permission handling)
- **Haptic feedback** — `navigator.vibrate()` on high-impact moments (fire, perform, battle)
- **Transition easing that matches the emotional arc** — elastic ease for "perform", heavy cubic for "fire", gentle sine for "mystery"
- **Colour grading shifts** — the overall colour temperature of the scene shifts across the narrative arc (warm → clinical → warm)
- **Film grain** — subtle, animated, photographic. Helps the 3D feel less "CG" on mobile screens.
- **A replay mechanism** — after the sequence ends, the organism idles beautifully and a "Watch Again" button appears.

---

## What Will Make This Fail

- **The organism is small or far away.** It must fill the phone screen. If there is significant empty black space around it, you have failed.
- **Dead air between cues.** Any silence longer than 0.5 seconds that isn't dramatically motivated is a failure.
- **Visual modes that look the same.** If "fire" and "battle" produce similar outputs, or "human" and "ai" are just colour swaps with no geometric difference, you have failed.
- **It feels like a slideshow.** If transitions between modes are abrupt state-swaps rather than continuous morphs, you have failed. Lerp everything.
- **Text is unreadable on mobile.** If the typography doesn't contrast against the 3D at every point in the sequence, you have failed.
- **It drops below 30fps on a mid-range phone.** GPU budget is real. If your geometry is too heavy or your shader too complex for mobile, scale it back intelligently — don't just ship a broken experience.
- **You used r170 or any outdated API.** If your research phase identified a newer version and you ignored it, you have failed.
- **You didn't audit the brief.** If your output just implements the brief as-written without questioning it, you have failed the meta-test.

---

## Evaluation Criteria

Your output will be evaluated on:

1. **Research quality** (20%) — Did you ground your decisions in current, verified information? Did you cite sources? Did you identify the right versions, APIs, and browser support?
2. **Brief audit depth** (20%) — Did you find real flaws? Did your amendments improve the specification? Did you demonstrate critical thinking about the brief itself?
3. **Visual impact** (25%) — Does it hit hard on a phone screen? Is the organism alive? Do the modes feel distinct? Is the typography integrated with the 3D?
4. **Technical execution** (20%) — Is the code clean? Does it handle edge cases? Is the architecture sound? Does it perform on mobile?
5. **Creative ambition** (15%) — Did you go beyond? Did you add ideas that elevate the experience? Did you surprise us?

The complete output should be a single HTML file that can be opened in any mobile browser and immediately delivers a cinematic, data-driven, voiceover-narrated 3D experience that makes the viewer stop scrolling.

---

## Final Note

This stress test is designed to separate models that execute instructions from models that think. A model that reads this brief and immediately starts writing HTML has already failed. A model that reads this brief, researches the current state of the art, identifies what's wrong with the brief, rewrites its own specification, and THEN builds something that exceeds all of it — that model passes.

The bar is not "does it work." The bar is "has anyone ever seen anything like this in a browser on a phone."
