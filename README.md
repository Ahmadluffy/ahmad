# Rovian Spring Water — Scroll-Driven 3D Landing Page

A cinematic, scroll-driven product landing page for **Rovian Spring Water** (premium 0.5L
natural spring water). Trilingual — English / Kurdish Sorani / Arabic — with full RTL
mirroring. Single self-contained page: no build step, no runtime CDN dependencies.

## Run

Serve the repo root over HTTP (locale JSON is fetched; an embedded fallback also covers
`file://`):

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## Stack

- **Three.js r160** (`vendor/three.module.min.js`, ES module via import map) — procedural
  bottle, `MeshPhysicalMaterial` transmission (PET ior 1.58 / water ior 1.33), GPU-instanced
  condensation, custom caustics shader, ACES filmic tone mapping.
- **GSAP 3.12 + ScrollTrigger** — one normalized scroll progress (0→1 over 620vh) drives the
  DOM copy timeline (scrubbed) and the 3D camera/bottle state machine.
- **Lenis 1.1** — smooth scroll inertia (disabled under `prefers-reduced-motion`).
- **i18n** — `locales/en|ku|ar.json`; the switcher swaps copy + `dir` live with a cross-fade;
  the 3D scene never reloads. RTL flips layout, slide directions, and the footer bottle side.

## Animation timeline

The full commented timeline map lives at the top of the module script in `index.html`.
Summary: hero (bob + 15s/rev spin) → 25–45% dolly-in to the ozone back label with 40° tilt
and lagging water slosh → 50–75% anticipated 360° spin with crown-splash apex, caustic surge
and light pulse → 90–100% pull-back, bottle settles on the CTA-opposite side at a ¾ angle.

## Scene graph (named meshes)

`Bottle_Shell`, `Bottle_Cap`, `Water_Volume` (animated meniscus vertices), `Label_Front`,
`Label_Back`, `Droplets_Instanced`, `Splash_Mesh` — all procedural (lathe profiles), so the
geometry is defined in code rather than shipped as a GLB.

## Performance / accessibility

Capped DPR (2 desktop / 1.75 mobile), reduced droplet & satellite counts on mobile,
`prefers-reduced-motion` fallback (fades only, no idle motion), semantic HTML behind the
canvas, no-WebGL and no-JS fallbacks, bounded font wait on the branded water-fill preloader.
