# Learning Progress

Concepts are marked learned based on **explained understanding**, not on code that happens to work.

Last updated: M0 — first sprite rendered.

## Unity

- [x] What a Unity project is on disk (`Assets` / `ProjectSettings` / `Packages` / `Library`)
- [x] `.meta` files and GUID-based asset references — [[meta-files-and-guids]]
- [x] Asset `Apply` vs scene `Cmd+S`; Play mode as a sandbox
- [x] Sprite import settings (PPU, filter mode, compression, mipmaps)
- [x] Pixels Per Unit and world units — [[pixels-per-unit]]
- [x] Orthographic camera size and integer scaling
- [x] Sprite sheet slicing, and why cells are padded — [[sprite-sheets-and-slicing]]
- [ ] GameObjects and Components *(used them; not yet explained back)*
- [ ] Scenes
- [ ] Prefabs
- [ ] MonoBehaviour and the Unity lifecycle
- [ ] `Update` vs `FixedUpdate`
- [ ] Rigidbody2D and Colliders
- [ ] Input System
- [ ] Tilemaps and Rule Tiles
- [ ] Animator Controllers
- [ ] Sorting layers / Y-sorting
- [ ] Coroutines
- [ ] ScriptableObjects
- [ ] Scene management
- [ ] Object pooling
- [ ] Save/load

## Game Development

- [x] Why pixel art needs point filtering and no compression
- [x] Integer scaling and why non-integer scaling shimmers
- [x] Uniform sprite cells as a stable animation anchor
- [ ] Game loop
- [ ] Frame-rate independence (`Time.deltaTime`)
- [ ] Collision detection
- [ ] Enemy AI / target selection
- [ ] Hitboxes vs hurtboxes
- [ ] Spawning and wave systems
- [ ] Game states
- [ ] Game balancing

## Practiced by doing

- Configured a texture importer from scratch and verified the result in the Editor
- Diagnosed a bad slice from its `W`/`H` values and re-sliced with `Delete Existing`
- **Tested a hypothesis in the Editor** (changed PPU to 32 to see which way the sprite moved) rather than guessing — keep doing this

## Needs more practice

- **Direction of the PPU relationship.** Got it right when tested, then applied it backwards in a differently-framed question. Anchor: *PPU up → sprite down*; PPU 1 would make sprites enormous.
- Vocabulary for things used but not yet named: GameObject vs Component vs Asset.

## Open questions to revisit

- Feet pivot vs center pivot for top-down depth sorting — deferred to M2
- Whether `ExtendedRuleTile.cs` (written for Tilemap Extras 4.x) compiles against the 6.0.2 we have
