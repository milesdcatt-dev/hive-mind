---
name: H&M branded loading screen
description: hmjunkland-site-v2 loader (logo-pill + mower) feature, files, debug params, and the baked-checkerboard asset gotcha
type: project
---

H&M marketing site (Desktop/hmjunkland-site-v2) has a branded full-screen loading
screen: the HM logo IS the pill (no extra pill), grass overlaps the pill's lower
edge, a mower character does a pull-start (3 frames) then thumbs-up, smoke + spark,
and a 5-step loading bar. Files: `loader.css`, `loader.js` (CONFIG/asset-path block
at top), markup inline at top of `index.html` `<body>`, assets in `assets/loader/`.

**Debug query params** (built into loader.js): `?loader=full` forces the full
animation (bypasses reduced-motion + once-per-session), `?loader=freeze` runs it
and holds the final frame, `?loader=off` skips it.

**HD quality source:** the original loader PNGs are low-res (logo pill only 402x117,
grass 640x155) so they blur when scaled up. A high-res logo lives in
`Downloads/hm_brand_assets_package.zip` → `01_logo_primary_horizontal.png` (1350x300,
cream bg == loader bg). The loader pill was rebuilt HD by drawing a green stadium
border (color #1F4D33 / (31,77,51), double-stroke) around that logo at 2x-supersampled,
giving `assets/loader/01_hm_logo_pill.png` at 1590x472. Mower frames (1388x993) are
already sharp. If quality needs improving again, source from the brand-assets zip.

**Why / How to apply — asset gotcha:** the loader source pack
(`hm_loader_animation_full_pack.zip`) shipped PNGs that were RGB with **no real
alpha** — a fake transparency *checkerboard was baked into the pixels* (~247/254
greys). They had to be keyed out (edge mask-flood + feather) before use, and the 4
mower frames re-cropped to a shared union bbox so frame-swaps don't jump. If Miles
sends more assets from this same generator, expect the same baked checkerboard and
key it out the same way rather than using them "directly".
