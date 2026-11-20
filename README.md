# Golden Hour Island

An interactive, real-time 3D product scene: the Golden Hour perfume bottle on a floating moss island in the sky.
Plain [three.js](https://threejs.org) r170 in a single `index.html` — no build step.

**Interact:** hover (or tap) the moss to make flowers bloom · move over / tap the bottle to slosh the liquid · drag to orbit · scroll to zoom.

## Run locally

The page fetches `env-sky.json`, so serve the folder rather than opening the file directly:

```bash
python3 -m http.server 8642
```

then open <http://localhost:8642>.

URL options: `?sky=hdri` shows the HDRI as the backdrop · `?glassdebug` shows the glass refraction diagnostics.

## What's in the scene

| Part | How it's made |
|---|---|
| Bottle | Your Cinema 4D model (`container2`, collar, dip tube), converted by `tools/convert_bottle.py` and embedded in the page |
| Glass | Two-sided image-space refraction (after Wyman 2005): back-face pass, refraction in and out, total internal reflection, per-channel dispersion, Beer–Lambert absorption of the liquid along the real path |
| Liquid | Fill level that stays level as the island tilts, damped-spring slosh, travelling waves, slow internal swirl, rising bubbles |
| Label | Your SVG, rasterised at 4×, on a sticker material: printed matte layer, edge bevel and grain normals, glossy clear-coat laminate |
| Moss | Shell texturing (stacked layers cut into strands) with hummocks, speckle and wind |
| Flowers | Procedural petals (cupped, curled, veined, translucent) with golden-angle centres; bloom on hover |
| Lighting | HDRI + strip softboxes for reflections, PCSS soft shadows, translucency on plants |
| Post | N8AO ambient occlusion, depth of field, bloom, neutral tone mapping, vignette, grain |

## Files

- `index.html` — the whole scene (bottle data embedded as base64)
- `env-sky.json` — the HDRI, 1024×512 Radiance RGBE as base64
- `label.svg` — label artwork
- `bottle.bin` — converted bottle geometry (source for the embedded copy)
- `tools/convert_bottle.py` — OBJ → `bottle.bin` (`python3 tools/convert_bottle.py perfume_bottle.obj bottle.bin`)
- `tools/shrink_hdr.py` — halves an `.hdr` and writes `env-sky.json`

## Credits

- [three.js](https://github.com/mrdoob/three.js) (MIT), including the `Sky` (Preetham) and `RGBELoader` addons
- [pmndrs/postprocessing](https://github.com/pmndrs/postprocessing) (Zlib)
- [N8AO](https://github.com/N8python/n8ao) (ISC)
- HDRI: [kloppenheim_07_puresky](https://polyhaven.com/a/kloppenheim_07_puresky) from Poly Haven (CC0)
- Fonts: Inter via Google Fonts (OFL)
- Bottle model and label artwork supplied by the project owner
