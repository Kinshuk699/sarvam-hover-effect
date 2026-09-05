# Sarvam Hover Effect

A single-file WebGL2 recreation of the **Sarvam sign-in texture** — a dense ASCII stitch fabric with the hover petal bloom — now rendering **album covers as living fabric**.

Every reload weaves a **random album cover** into the cloth (63 covers in rotation, self-contained in the HTML). Hover: tiles near your cursor **sprout outward like petals opening**, each rising in 3D (far edge pitched up), elongating into tilted rectangles with tapered tips, then flattening as you move away. Click does nothing — exact event parity with the original.

## Run it

```bash
open index.html
```

Works in any WebGL2 browser, fully offline — covers and font are inlined (1.5 MB single file).

## The album fabric

- **Random cover per load** — never the same twice in a row (`?cover=N` forces one; the **NEXT COVER →** button cycles live)
- **Full square cover, centered** — the entire artwork visible, with the surrounding fabric continuing as a stretched bleed of the cover's own colors (seam blended)
- **Weave-in on load** — the new cover un-sews and re-stitches left-to-right with stagger (the "refreshing effect")
- **True colors per stitch** — each cell keeps the cover's actual pixel color; the ASCII glyph ramp (`·:-=+*#%@`) follows luminance

## How it works

- **Rendering**: WebGL2 instanced quads, one per stitch (8px cell pitch, ~20k instances, single draw call)
- **Physics**: spring-network cloth (spring constants, damping and pointer stir translated from the original engine's `physics: { mode: "cloth", spring: 14, radius: 80 }` config)
- **Hover bloom**: per-stitch lift channel driven by pointer proximity (radius 80). In the vertex shader each lifted tile orients radially away from the cursor (organized starburst), elongates ~2.5:1, and pitches up — far edge rises ~7px higher than the near edge (the 3D tilt)
- **Shading**: light-from-above bevel, top-left rim highlight, underside shadow; ASCII glyph ramp overlaid for the stitch texture
- **Recovery**: tiles re-sew left-to-right with stagger after you release — from the original's `motion: { order: "ltr", stagger: 120 }` config

## Status

Hover-bloom model + album-cover pipeline complete: petal sprout, 3D pitch, ruffle variance, gap separation, no black-on-hover, no lattice at rest, centered full-cover layout with bleed. Next: port into the KinshukWebsite hero.

Inspired by [indus.sarvam.ai](https://indus.sarvam.ai/) — reverse-engineered from its public engine bundle for learning purposes.
