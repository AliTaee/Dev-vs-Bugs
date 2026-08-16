# Decision 002: Pixel-Art Import Settings

**Date:** 2026-08-16 · **Status:** accepted

## Decision

Every texture imported into this project uses:

| Setting | Value |
|---|---|
| Texture Type | `Sprite (2D and UI)` |
| Sprite Mode | `Multiple` (for sheets) |
| **Pixels Per Unit** | **`16`** |
| **Filter Mode** | **`Point (no filter)`** |
| **Compression** | **`None`** |
| Generate Mipmaps | off |
| Mesh Type | `Tight` · Extrude Edges `1` |

Camera: orthographic, **Size `5.625`** (a 320×180 view), Game view **16:9**.

## Why

These aren't preferences — three of them are Unity defaults that are actively wrong for pixel art.

**PPU 16** matches the tile grid, so **one tile = one world unit**. Every later gameplay number then reads as a count of tiles (`speed = 4` → four tiles/second). Unity's default of 100 would make a tile 0.16 units and turn every number into noise. It also must be *uniform*: mixing PPU across textures renders art at wrong relative scales. See [[pixels-per-unit]].

**Point filtering** — Bilinear (the default) blends neighbouring pixels when scaling. Correct for photographs, fatal for pixel art; it is precisely the blur we're avoiding.

**No compression** — block compression (DXT/ETC) is lossy and produces colour artifacts that are glaring on a hand-picked palette. These textures are a few KB each; there is nothing to save.

**Camera size 5.625** gives a 320×180 view, which divides evenly into 1920×1080 (exactly 6×). **Integer scaling** keeps every pixel the same size on screen; non-integer scaling makes some pixels 6 wide and others 7, producing shimmer.

## Corroboration

These match the values the artist shipped in their own `.unitypackage` `.meta` files — independently confirming PPU 16 and Point filtering are what the art was authored for:

```yaml
spritePixelsToUnits: 16
filterMode: 0          # Point
enableMipMap: 0
spriteExtrude: 1
spriteMeshType: 1      # Tight
```

## Future improvement

Add URP's **Pixel Perfect Camera** component to enforce integer scaling at *any* window size. Currently the 6× ratio only holds at 1080p; resize the window and pixels go uneven again. Deliberately deferred so the problem is visible before the fix is applied.

## Related

- [[pixels-per-unit]] · [[sprite-sheets-and-slicing]] · [[001-project-layout]]
