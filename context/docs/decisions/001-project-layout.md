# Decision 001: Project Layout

**Date:** 2026-08-15 · **Status:** accepted

## Decision

The repository root is a *workspace*, not the Unity project. The Unity project lives in `game/`, and the raw art packs live in `art-source/` — outside it.

```
Dev vs Bugs/
├── context/            # Obsidian knowledge base (docs/ + knowledge/)
├── art-source/         # raw packs — NOT imported by Unity
│   └── Pixel Farm/
└── game/               # ← Unity project root
    ├── Assets/
    ├── Packages/
    └── ProjectSettings/
```

## Why

**The forcing issue: `assets/` would have collided with `Assets/`.** macOS APFS is case-insensitive, so rooting the Unity project here would have made Unity silently adopt the existing `assets/` folder as its own `Assets/`, pulling 603 raw PNGs, 43 `.aseprite` files, and 3 `.unitypackage` files into the import pipeline.

Beyond avoiding the collision:

- **Unity imports everything under `Assets/`, whether used or not.** Each import costs time and generates `Library/` entries. We use a small fraction of 603 sprites; there's no reason to pay for the rest.
- **`.unitypackage` files don't belong inside `Assets/`** — they're archives to be imported, not assets.
- **Keeping the original packs pristine** gives us a reference copy that Unity's importer never touches, so we can always re-copy a sprite and compare.

Art is copied into `game/Assets/Art/…` only when we actually use it.

## Alternatives considered

**Unity project at the repo root.** Fewer nested folders, but `assets/` still has to be renamed, and raw art plus Unity project share a root — muddier, with no real gain.

**Import the whole `Pixel Farm` pack into `Assets/`.** Everything available immediately, at the cost of importing ~600 unused sprites and putting `.unitypackage` archives somewhere they don't belong.

## Consequences

- Adding art is a deliberate copy step, not automatic. Slightly more friction, which is the point.
- `art-source/` **is committed** (7.2 MB). ⚠️ The Otterisk license forbids redistribution — **this repo must not be pushed public** without stripping the art first.

## Related

- [[002-pixel-art-import-settings]]
- [[asset-reference]]
