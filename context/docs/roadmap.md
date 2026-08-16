# Roadmap

**Objective:** the smallest *fun* playable version of the core loop, built in a way I understand.

> Farm → Prepare → Bugs Attack → Defend → Earn Resources → Improve Farm → Repeat

## Shape of the game (decided at kickoff)

- **Action combat** — walk around and swing a farm tool at bugs in real time. Not tower defense. Reuses the Farmer's existing swing animations and teaches input/physics/collision, which are prerequisites either way.
- **Placeholder bug art** — coloured shapes until the loop proves fun. See the gap in [[asset-reference]].

## Minimum Playable Prototype

1. Farmer walks around a small fenced field (animated, 4 facings)
2. ~6 fixed planting plots — no tilling system, no tilemap editing
3. **One** crop type, growing through its authored sprite stages on a timer
4. Harvest → `+1` of a single resource
5. Day phase (farm freely) → Night phase (bugs come) → repeat
6. **One** bug type: walks toward the nearest crop and destroys it
7. Swing a sickle → bug dies in 1–2 hits
8. **Spend the resource to buy a seed** — this is what closes the economy loop
9. Win: survive 3 nights. Lose: all crops destroyed.

**Not in the prototype:** tilling/watering, seasons, day/night lighting, multiple crops, inventory, save/load, livestock, fishing, NPCs, dialogue, sound, menus, ScriptableObjects, object pooling, A* pathfinding, state-machine frameworks, event buses.

> Item 8 is the one people cut and shouldn't. Without spending the resource, harvesting has no purpose and the "loop" is a straight line.

## Milestones

### ✅ M0 — Foundations & Project Setup
Unity 6.3 Universal 2D project in `game/`; git + Unity `.gitignore`; `assets/` → `art-source/`; pixel-art import settings locked; first sprite rendering at true pixel scale.
**Learned:** [[pixels-per-unit]] · [[meta-files-and-guids]] · [[sprite-sheets-and-slicing]]
**Remaining:** import the tileset `.unitypackage`, draw a grass field with Rule Tiles, add the Pixel Perfect Camera.
**Done when:** a grass field renders crisply at true pixel scale.

### M1 — The Farmer Moves
Rigidbody2D + collider sized to the *visible body*; `PlayerMovement` using the new Input System; collision against obstacles; a small camera-follow script.
**Unity:** MonoBehaviour · `Update` vs `FixedUpdate` · `Time.deltaTime` · `Vector2` · Rigidbody2D · colliders · `[SerializeField]` · Input Actions
**Gamedev:** frame-rate independence · normalizing diagonal movement
**Done when:** walk in 8 directions at consistent speed, blocked by obstacles, camera trailing.

### M2 — Animation & Facing
Animator driven by facing direction; flip the side row for left; Y-based depth sorting.
**Unity:** AnimationClip · Animator Controller, parameters, transitions · `flipX` · Sorting Layers and Y-sorting · sprite pivots
**Gamedev:** animation state driven by game state, not the reverse · depth sorting in top-down games
**Done when:** the farmer idles and walks in four facings and sorts correctly against props.

### M3 — Farming: Plant → Grow → Harvest
A `Plot` prefab placed ~6 times; a `Crop` script advancing through the authored 16×32 stage sprites on a timer; interact key; resource counter on screen.
**Unity:** Prefabs · `Instantiate`/`Destroy` · trigger colliders · swapping `SpriteRenderer.sprite` at runtime · coroutines vs a float timer · TextMeshPro
**Gamedev:** growth stages as a data sequence · the interaction verb · designing around a resource
**Done when:** plant, watch it grow through every stage, harvest, counter increments.

### M4 — Bugs Exist
Placeholder bug prefab; edge spawner; bug seeks the nearest crop and destroys it after a chew delay.
**Unity:** runtime instantiation · Tags and Layers · `Vector2.MoveTowards` · `OnDestroy`
**Gamedev:** target selection · simplest possible enemy AI · spawn points
**Done when:** a bug walks in unopposed and eats a crop, and losing it feels bad.

### M5 — Defense
Attack on key press: play the swing, then `Physics2D.OverlapCircle` in front of the farmer. Bug health and death; player health; hit flash.
**Unity:** Layer Collision Matrix · `OverlapCircle` vs triggers · `Gizmos` for visualizing hitboxes · coroutines for timed feedback
**Gamedev:** hitboxes vs hurtboxes · attack timing (windup/active/recovery) · i-frames · why feedback beats damage numbers
**Done when:** you can kill bugs before they reach the crops, and you can also lose.

### M6 — The Loop
Day → Night phase timer; escalating waves; buy seeds with the resource; win/lose; minimal UI.
**Unity:** a plain `enum` game state (deliberately *not* a state-machine framework) · coroutines for sequencing · scene reloading · Canvas and anchoring
**Gamedev:** the game loop as a closed economy · pacing and tension · wave design · first-pass balancing
**Done when:** a stranger can launch it, understand it unaided, and reach a win or lose screen. **This is the prototype.**

### M7 — Only if it's fun *(not committed)*
Real bug art, SFX, screen shake, particles, more crops, tool upgrades. Also where deferred "future improvements" — ScriptableObjects for crop/bug data, object pooling, a proper state machine — get revisited **if the code has actually started hurting**.

## Related

- [[asset-reference]] · [[001-project-layout]] · [[002-pixel-art-import-settings]] · [[learning-progress]]
