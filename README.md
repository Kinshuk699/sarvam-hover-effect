# Sarvam Hover Effect

A single-file WebGL2 recreation of the **Sarvam sign-in texture** — the dense ASCII-looking stitch fabric with the hover bloom.

Hover your cursor over the landscape: tiles near the pointer **sprout outward like petals opening**, each one rising in 3D (far edge pitched up), elongating into tilted rectangles with tapered tips, then flattening as you move away. Click does nothing — exact event parity with the original.

## Run it

```bash
open index.html
```

Or serve it:

```bash
python3 -m http.server 8080
# → http://localhost:8080
```

Works in any WebGL2 browser (Chrome, Safari, Firefox). Fonts are self-hosted in `assets/` — no network needed.

## How it works

- **Rendering**: WebGL2 instanced quads, one per stitch (8px cell pitch, thousands of instances, single draw call)
- **Physics**: spring-network cloth (spring constants, damping and pointer stir translated from the original engine's `physics: { mode: "cloth", spring: 14, radius: 80 }` config)
- **Hover bloom**: per-stitch lift channel driven by pointer proximity (radius 80). In the vertex shader each lifted tile:
  - orients radially **away from the cursor** (organized starburst)
  - elongates ~2.5:1 and shrinks across (opens gaps, no collisions)
  - pitches up — the far edge rises ~7px higher than the near edge (the 3D tilt)
- **Shading**: light-from-above bevel, top-left rim highlight, underside shadow; ASCII glyph ramp (`·:-=+*#%@`) overlaid at low alpha for the stitch texture
- **Recovery**: tiles re-sew left-to-right with stagger after you release — the "refreshing" sweep from the original's `motion: { order: "ltr", stagger: 120 }` config

## Status

Hover-bloom model complete: petal sprout, 3D pitch, ruffle variance, gap separation, no black-on-hover, no lattice at rest. Next: port into the KinshukWebsite hero.

Inspired by [indus.sarvam.ai](https://indus.sarvam.ai/) — reverse-engineered from its public engine bundle for learning purposes.
