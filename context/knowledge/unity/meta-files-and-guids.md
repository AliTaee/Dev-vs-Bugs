# .meta Files, GUIDs, and What Actually Gets Saved

**Status:** learned — reasoned through correctly at the M0 checkpoint

## Every asset has a `.meta` sibling

Every file under `Assets/` gets a companion `.meta` containing:

- a **GUID** — a permanent unique id for that asset
- that file's **import settings** (PPU, filter mode, sprite slices, …)

## Unity references assets by GUID, not by path

Our scene doesn't say *"use `farmer - idle.png`"*. Read the raw scene file and it says:

```yaml
m_Sprite: {fileID: -6551302418908332334, guid: 7bbaa70ccac554a22a2e0f099bbe1f27, type: 3}
```

The filename appears **nowhere**. `guid` identifies the texture; `fileID` identifies which sliced sub-sprite within it.

This is why you can freely rename and move assets *inside* the Editor — the path changes, the GUID doesn't, so every reference survives.

## The failure mode

Rename `farmer - idle.png` → `farmer-idle.png` **in Finder**, then reopen Unity:

1. `farmer - idle.png.meta` is left orphaned — its PNG no longer exists
2. Unity finds `farmer-idle.png` with no `.meta` and mints a **brand new GUID**
3. The scene still asks for GUID `7bbaa70c…`, which now belongs to nothing
4. The `Sprite` field reads **`Missing`** and the object renders as *nothing*

The GameObject and its SpriteRenderer survive — only the link dies. That's why it shows up as an invisible object rather than a loud error, which makes it easy to miss.

> **Rules:** rename and move inside the Editor. Always commit `.meta` files. If you must act outside Unity, move the `.meta` identically — they travel as a pair.

Copying a *new* file **into** `Assets/` from outside is safe: there's no `.meta` yet, so Unity just creates one.

## Two different kinds of "save"

Bit us during M0 — the scene looked right on screen but the file on disk was unchanged.

| Change | When it hits disk |
|---|---|
| Import settings, sprite slices | Immediately on **`Apply`** |
| Scene contents (objects, components, transforms) | Only on **`Cmd+S`** |

An unsaved scene shows a `*` in the title bar.

**Play mode is a sandbox:** edits made while playing are discarded when you stop. Tweaking values during Play to find a good number is a great workflow — just write the number down before pressing stop.

## What belongs in git

| Folder | Commit? | Why |
|---|---|---|
| `Assets/` | ✅ | Everything you author (plus its `.meta` files) |
| `ProjectSettings/` | ✅ | Physics, tags, layers, render pipeline |
| `Packages/manifest.json` | ✅ | The dependency list — Unity's `package.json` |
| `Library/` | ❌ | Generated import cache; rebuilt automatically |
| `Temp/`, `Logs/`, `UserSettings/` | ❌ | Transient / machine-local |

`Library/` is `node_modules` plus a build directory. "Delete `Library/` and reopen" is a standard Unity repair step — that's how disposable it is.

## Related

- [[pixels-per-unit]]
- [[sprite-sheets-and-slicing]]
