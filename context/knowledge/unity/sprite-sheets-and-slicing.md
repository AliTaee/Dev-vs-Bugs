# Sprite Sheets and Slicing

**Status:** learned — needed a second explanation on *why* cells are padded

## What slicing does

A sprite sheet is **one image file containing many poses**. `farmer - idle.png` is a single 320×240 picture that happens to hold twelve farmers.

Unity can't infer that. **Slicing** writes rectangles into the `.meta` saying "a sprite lives here, and here, and here" — turning one asset into twelve addressable sprites.

Without slicing, dragging the file into a scene gives you one object showing all twelve farmers at once.

## How to do it

Inspector → **`Sprite Mode: Multiple`** → **`Open Sprite Editor`** → in *that window's* toolbar, **`Slice ▾`**:

- **Type:** `Grid By Cell Size` (a dropdown selection, not typed text)
- **Pixel Size:** X `80`, Y `80`
- **Method:** `Delete Existing` when re-slicing, to wipe previous rects

Then **`Slice`**, then **`Apply`** in the toolbar. **`Slice` alone does not save** — closing without `Apply` silently discards everything.

⚠️ **Don't use `Trim` or `Automatic`** on character sheets. Both shrink rects to fit visible pixels, which is exactly what breaks animation (see below).

**Verify:** select any sprite; it must read `W 80  H 80`. The grid should tile the whole image edge-to-edge with equal squares. Boxes hugging each character = wrong slice type.

## Why cells are padded (the important part)

The farmer is **13×16 px** inside an **80×80** cell — ~94% empty. That padding is not waste; it buys a **stable anchor**.

Unity positions a sprite by its **pivot**, which defaults to the center *of the cell*. The cell center is the point nailed to the GameObject's position.

With tight boxes, the box grows to contain the swinging axe — and the axe isn't centered on the body. So the box's center lands somewhere different every frame. Measured drift on the actual art:

| Animation (side view) | tight-box center drift | world units at PPU 16 |
|---|---|---|
| axe | 10.0 px | **0.62** |
| sickle | 7.5 px | 0.47 |
| watering | 5.0 px | 0.31 |

The farmer is **0.8 units** tall. Tight boxes would make him lurch ~¾ of his own body per swing — sliding around instead of standing and swinging.

With a uniform cell, the artist draws the body in the same spot every frame, so the pivot always lands on the same part of him and only the limbs and tool move.

> **The padding is what makes the anchor stable.** Uniform cells → frames align. That's the whole reason.

## Our character sheets

- Cell **80×80**; `walk` = 8 columns, most actions = 6, `idle` = 4
- **3 rows = `side` / `down` (front) / `up` (back)** — left is the side row flipped horizontally
- Slice order is left-to-right, top-to-bottom: `_0.._3` side, `_4.._7` front, `_8.._11` back
- Largest content anywhere in the pack is 33×22 px, so 80 is generous headroom, not a tight fit. It's also 5 × 16 — a whole number of tiles.

**Pivot is still `Center`.** For top-down depth sorting we'll likely want a *feet* pivot instead — the visible body sits at roughly `(0.49, 0.41)` normalized. Deferred to M2.

## Related

- [[pixels-per-unit]]
- [[meta-files-and-guids]]
