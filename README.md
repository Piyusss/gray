# Binary Grey

A full-screen field of flickering `0`s and `1`s that resolves into a three-dimensional
Grey. Move the pointer to sweep the light across the face and magnify the digits
underneath it.

One file. No build, no dependencies, no framework — open `index.html` in a browser.

## How it works

The figure is not a traced image or a flat mask. It is a **height field**: for every point
on the picture plane there is a depth `z(x, y)` describing how far the surface stands
toward the viewer. Brightness comes from the surface normal — the gradient of that depth
map — so the volume is real geometry rather than painted-on shading.

**The skull** is an elliptical dome above its widest point and a concave taper below it,
which is what produces the inverted-teardrop cranium. Its front-to-back depth is a
separate profile from its width — `1.12 ×` the half-width at the cranium easing to
`0.78 ×` at the jaw. A deep bulging skull and a shallow face.

**The eyes** are convex lenses, not holes. They stand `0.075` units proud of the skull with
an albedo near zero and a tight `spec⁶⁴` highlight, so they stay black but carry a glint
that slides around as the light moves. Their outline is an almond whose width follows
`s^0.55 · (1−s)^0.38`, peaking 59% of the way outboard — a pointed inner tip with the bulge
toward the temple.

**The rest** is a shallow brow shelf, a faint nasal ridge in place of a nose, nostril pits
and a mouth crease cut into the surface, then a cylindrical neck, rounded shoulder caps,
two collarbone ridges and a sternum hollow.

Every facial feature is clipped against an inset copy of the skull, so nothing can eat
through the silhouette at any viewport size.

## Performance

Geometry and normals are baked once per resize into typed arrays. Each frame only
relights them, which is a dot product per cell — so a point light can follow the cursor
in real time. Cells are bucketed by brightness with a counting sort, so a frame sets
`fillStyle` about 48 times instead of once per digit; without that, a full-screen field
at this density stutters badly.

The grid pitch scales with the figure (`S / 34`, clamped to 9–16 px), keeping roughly
23–38 digits across the head on a phone or a 4K display alike.

Respects `prefers-reduced-motion` — the figure still renders, it just holds still.
Works with touch as well as a mouse.

## Deploying

It is a static file, so anything that serves HTML will do.

**GitHub Pages** — Settings → Pages → deploy from branch → `main` / root. `index.html` is
served automatically.

**Vercel**

```bash
npx vercel --prod
```

No configuration needed; it is detected as a static site.
