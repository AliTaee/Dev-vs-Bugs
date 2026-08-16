# Asset Reference

Measured facts about the art in `art-source/Pixel Farm/`. Five packs by **Otterisk** (Hana Caraka series).

> **License:** commercial and non-commercial use and modification allowed. **Resale, repackaging, or redistribution is not** — including in a public repo. Credit appreciated, not required.

## Grid sizes

| Art | Cell |
|---|---|
| Tiles | **16×16** |
| Interior props | **8×8** (slice on multiples of 8) |
| Characters | **80×80** |
| Crops | **16×32** |

All of it imports at **PPU 16** → [[002-pixel-art-import-settings]]

## Characters — `Hana Caraka - Base Character`

`Base Character/` is a **bald, unclothed base** meant for layering clothing. Use `Premade Character/` instead unless we build a layering system.

**Player = the Farmer** — thematically right (dev-turned-farmer) and by far the most complete set:

| Character | Sheets | |
|---|---|---|
| **Farmer** | **13** | axe, brush, hoe, idle, milking, pick up, planting, run, shear, shovel, sickle, walk, watering |
| Angler | 8 | + fishing set |
| Miner | 6 | + hammer, pickaxe |
| Blacksmith / Chef / Florist / Inn Keeper / Merchant | 4 | idle, walk, run, pick up only |

**Sheet layout:** 80×80 cells · `walk` = 8 columns, most actions 6, `idle` = 4.
**3 rows = `side` / `down` (front) / `up` (back).** Left is the side row flipped. Slice order left-to-right, top-to-bottom → `_0.._3` side, `_4.._7` front, `_8.._11` back.

**Visible body ≈ 13×16 px, centred in the 80×80 cell** (spans x 33–46, y 32–47). Largest content anywhere in the pack is 33×22 px.

⚠️ **Colliders must be sized to the ~0.8-unit body, not the 5-unit cell.**
⚠️ **The Farmer has no attack animations** — sword/bow/spear exist only on the unclothed base. Mitigation: fight bugs with `sickle` / `axe` / `hoe`, which fits the theme better anyway.

Feet pivot for depth sorting would be ≈ `(0.49, 0.41)` normalized. Currently `Center`; revisit in M2.

## Crops — `Hana Caraka - Farming n Foraging`

**The best-fitting asset in the project.** 30 crops, each a **16×32 strip of 5–7 growth stages** — plant/grow/harvest is already drawn. Widths: 80px = 5 stages, 96px = 6, 112px = 7.

Also: `misc.png` (tilled soil tiles, seed bags, tools, greenhouses), `sprinkler.png`, `info.png` (a contact sheet showing every crop's seed icon, harvested item, and stages), plus berries / flowers / herbs / mushrooms.

## Tiles — `Hana Caraka - Topdown Tileset`

Seasonal terrain (spring/summer/fall/winter/sand/dirt), water incl. waterfalls, paths, house, fences, bridges, rocks, props. Terrain sheets are 144×96 (9×6 of 16px) in an **auto-tile blob layout** — meant for Rule Tiles, not manual placement.

### Use the `.unitypackage`, don't slice by hand

`Unity 2022 LTS Demo/Hana Caraka - Topdown Tileset [v2.1].unitypackage` contains **pre-built Rule Tile assets**, saving hours:

| Package | Tile `.asset`s | Sprites |
|---|---|---|
| Topdown Tileset **v2.1** | **736** | 154 |
| Fantasy Interior | 239 | 83 |
| Topdown Tileset v2.0 | 206 | 96 |

Each also ships a Tile Palette prefab, a demo scene, and `ExtendedRuleTile.cs`.

⚠️ **Import only v2.1** — `ExtendedRuleTile.cs` ships identically in all three at the same path; importing several risks duplicate-class compile errors.
⚠️ Authored for **Unity 2022 LTS / Built-In RP**; we're on Unity 6.3 / URP. Sprites and tiles should import fine, but the bundled **demo scenes may render magenta**. Import for the tiles; ignore the demo scenes.
⚠️ `ExtendedRuleTile.cs` targets **Tilemap Extras 4.x**; we have **6.0.2**. Compatibility unverified.

## Other packs

- **Fantasy Interior** — floors, walls, structures, furniture, animated props. Not needed for the prototype.
- **Livestock & Fish** — chicken, cow, duck, goat, horse, pig, sheep, with young variants. Out of prototype scope.

## ⚠️ The gap: there is no bug art

A search of all 603 PNGs for bug/insect/beetle/ant/spider/worm/bee/pest/enemy/monster returns **nothing**. The only non-livestock creature is `mole.png` — a 288×32 decorative pop-out animation with `?` bubbles and no walk cycle.

For a game called *Dev vs Bugs*, **the antagonist does not exist as art.** Prototype uses placeholder coloured shapes; real sprites only once the loop proves fun.

## Related

- [[001-project-layout]] · [[002-pixel-art-import-settings]] · [[sprite-sheets-and-slicing]]
