# Pixels Per Unit (PPU)

**Status:** learned — verified by experiment (changed PPU to 32 in the Editor and observed the result)

## What it is

Unity simulates the world in abstract **world units**, not pixels. PPU is the conversion rate between the two:

```
world units = pixels ÷ PPU
```

PPU is a **per-texture import setting**, but in practice it must be the *same across the whole project*.

## The direction (the part that's easy to get backwards)

PPU is the **divisor**. "Pixels per unit" = how many pixels are packed into one unit. Pack more in, each pixel takes less space.

> **PPU up → sprite down.**

Sanity check when in doubt: PPU 1 means *one pixel = one whole world unit*, which makes sprites enormous. Low PPU = giant, high PPU = tiny.

## Our project: PPU 16

Pegged to the **16px tile grid**, not to any sprite's canvas size.

| Art | pixels | ÷ 16 = units |
|---|---|---|
| Tile | 16×16 | **1 × 1** |
| Crop | 16×32 | 1 × 2 |
| Character cell | 80×80 | 5 × 5 |
| Character *visible body* | 13×16 | ≈ 0.8 × 1.0 |

**One tile = one world unit** is the whole point. It makes every later number readable as a count of tiles:
- `position = (3, 5)` → three tiles right, five tiles up
- `speed = 4` → four tiles per second

At PPU 100 (Unity's default) a tile would be 0.16 units and every gameplay number becomes noise.

## Why it must be consistent

PPU is the shared scale between *all* art. Mix them and things render at wrong relative sizes:

| | PPU 16 ✓ | PPU 80 ✗ |
|---|---|---|
| 13px farmer body | 0.81 units | 0.16 units |
| next to a 1-unit tile | ~⅘ of a tile | ~⅙ of a tile |

Same art, same tiles — the farmer just becomes an ant. **Every texture we import gets PPU 16.**

## Orthographic camera size

2D cameras are **orthographic** — no perspective. Their `Size` field = **half the visible height, in world units**.

```
Size 5.625  →  11.25 units tall  →  × 16 PPU  =  180 pixels
```

We chose 5.625 to show a **320×180** view, because that divides evenly into 1920×1080 (exactly 6×). **Integer scaling** keeps every pixel the same size on screen; non-integer scaling makes some pixels 6 wide and others 7, producing the shimmer that makes pixel art look cheap.

General formula:

```
Size = (target vertical resolution ÷ 2) ÷ PPU
```

## Mental model

PPU is a **map scale** — "1 cm = 1 km". It doesn't change the territory or the paper, only the conversion between them. The art is the map; world units are the territory.

## Related

- [[meta-files-and-guids]]
- [[sprite-sheets-and-slicing]]
- Decision: [[002-pixel-art-import-settings]]
