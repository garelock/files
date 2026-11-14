# Animation & Movement Guide — source-derived asset map

This guide maps the supplied sprites to game states. It does not invent missing animations. File paths are relative to the kit root.

## Player: Body_A
The matching Aseprite files confirm 64×64 px frames. PNG sheets are horizontal strips. Play frames left-to-right. Matching project files report 100 ms per frame for the examples inspected; use each exact per-file duration array in `asset_manifest.json` when available.

| Game state / action | Available source animation | Direction variants | Suggested use |
|---|---|---|---|
| `Idle_Base` | `Idle_Base` | Down, Side, Up | Stand still; loop gently while no movement input is active. |
| `Walk_Base` | `Walk_Base` | Down, Side, Up | Normal movement; loop while moving at walking speed. |
| `Run_Base` | `Run_Base` | Down, Side, Up | Faster movement; loop while sprint input is active. |
| `Carry_Idle` | `Carry_Idle` | Down, Side, Up | Idle while holding/carrying an object. |
| `Carry_Walk` | `Carry_Walk` | Down, Side, Up | Walk while carrying an object. |
| `Carry_Run` | `Carry_Run` | Down, Side, Up | Run while carrying an object. |
| `Collect_Base` | `Collect_Base` | Down, Side, Up | One-shot interaction for picking up/gathering an item; return to idle after completion. |
| `Fishing_Base` | `Fishing_Base` | Down, Side, Up | Fishing interaction; use near water, play as a one-shot or controlled loop while waiting. |
| `Watering_Base` | `Watering_Base` | Down, Side, Up | Watering interaction; play when watering a crop or plant. |
| `Slice_Base` | `Slice_Base` | Down, Side, Up | Melee slash attack; play once and trigger hit detection during the active attack frames. |
| `Pierce_Base` | `Pierce_Base` | Down, Side, Top | Thrust/piercing attack; play once and trigger a narrow forward hit area. |
| `Hit_Base` | `Hit_Base` | Down, Side, Up | Damage reaction; briefly override normal movement animation after taking damage. |
| `Death_Base` | `Death_Base` | Down, Side, Up | Death sequence; play once, then disable player control or trigger respawn flow. |
| `Crush_Base` | `Crush_Base` | Down, Side, Up | Crush/impact action; treat as a contextual one-shot, not a normal locomotion loop. |

### Direction selection
- `Down`: character faces toward the bottom of the screen (toward the camera).
- `Up` or `Top`: character faces away from the camera. The `Pierce_Base` sheet uses the filename `Pierce_Top-Sheet.png`; treat it as the upward-facing variant.
- `Side`: use for horizontal movement. Flip the sprite horizontally for the opposite horizontal direction if the artwork is symmetrical and the engine supports sprite flipping.
- If a direction variant is missing for a particular action, use the closest available directional variant rather than inventing a new sheet.

### Movement state machine
1. If dead: play `Death_Base` once and stop other animation updates.
2. If damaged: play `Hit_Base` briefly, then return to the movement state.
3. If an attack input is accepted: play `Slice_Base` or `Pierce_Base` once; lock conflicting actions until its attack window ends.
4. If interacting with a pickup: play `Collect_Base` once.
5. If fishing or watering: play the corresponding action animation while the interaction is active.
6. Otherwise, if carrying: choose `Carry_Idle`, `Carry_Walk`, or `Carry_Run` according to movement speed.
7. Otherwise, if moving: choose `Walk_Base` or `Run_Base` according to movement speed.
8. Otherwise: choose `Idle_Base`.

### Timing and frame slicing
- Body_A frame cell: 64×64 pixels. For a horizontal sheet, number of frames = PNG width ÷ 64.
- The matching Aseprite project metadata is recorded per asset in `asset_manifest.json`; use the listed `frame_durations_ms` rather than guessing when present.
- If a runtime uses a single fixed FPS, start with 10 frames per second for source animations whose Aseprite timing is 100 ms/frame.
- Preserve transparent pixels and nearest-neighbor scaling. Disable texture smoothing/linear filtering to keep pixel edges crisp.
- Keep the sprite origin/feet anchor consistent across frames so the character does not jitter.

## Enemies: Orc Crew and Skeleton Crew
The supplied enemy folders include `Idle`, `Run`, and `Death` sheets for the base orc, orc rogue, orc shaman, orc warrior, skeleton base, skeleton mage, skeleton rogue, and skeleton warrior. Use `Idle` as a loop when stationary, `Run` as a loop while pursuing, and `Death` as a one-shot when defeated. The pack names no explicit attack animation for these mobs; implement attacks through game logic and use a brief approach/contact effect rather than claiming a dedicated attack sheet exists.

## NPCs
The supplied NPC assets include Peasant_A (`Idle`, `Walk`), Tavern_A and Tavern_B (side-facing `Idle`, `Idle_Hold`, `Walk`, `Walk_Hold`), plus Knight, Rogue, and Wizzard (`Idle`, `Run`, `Death`). Use the available animation variants only; for NPCs without a Walk sheet, use a slower `Run` playback or keep them stationary rather than fabricating a walk cycle.

## Environment animation
The pack contains five `Pan_01` through `Pan_05` animated prop sheets, plus animated station sheets such as bonfire/fire/smoke and cooking/workstation assets. Use manifest paths and inspect each sheet before slicing. Do not animate static prop atlases as if each tile were a frame.

## Asset mapping rules
- Terrain: use `Environment/Tilesets/` for floor, wall, water and dungeon tile atlases.
- Village/buildings: use `Environment/Structures/Buildings/` and `Environment/Structures/Buildings/Interior/`.
- Crafting stations: use `Environment/Structures/Stations/` (Alchemy, Anvil, Bonfire, Cooking Station, Furnace, Sawmill, Workbench).
- Nature and props: use `Environment/Props/Static/`, including Trees, Vegetation, Farm, Resources, Rocks, Furniture and Tools.
- Weapons: use `Weapons/` sheets as source artwork; inspect atlas layout before selecting individual items.
- `MockUps/Tavern.png` is a visual reference/mockup, not a tile atlas to slice blindly.

## What the asset pack does not prove
- It does not provide a complete game engine, collision map, quest logic, inventory logic, UI, or save system.
- The mob animation names do not show dedicated attack cycles.
- Do not claim every NPC has four-direction animation; several are side-facing only.
- Do not invent frame counts or cell dimensions for sheets unless read from metadata or visually confirmed.