# Extraction Zone — Game Design Document

Last updated: 2026-08-16  
Project repo: `ExploiterMpg/jubilant-enigma`  
Current implementation entry point: `app/index.html`

## 1. High concept

Extraction Zone is a lightweight browser-based tactical extraction shooter inspired by the tension, looting, and inventory pressure of games like Escape from Tarkov.

The player prepares gear in a stash, deploys into Ezra Town, searches buildings and containers, fights hostile Scavs, finds better loot, and attempts to extract alive. The main fantasy is not “clear the map”; it is “survive long enough to leave with something worth keeping.”

Core loop:

```text
Stash → Prepare → Raid → Search → Loot → Fight → Extract → Upgrade
```

## 2. Design pillars

### 2.1 Extraction tension

Every raid should create pressure through limited visibility, hostile Scavs, slow looting, reload/heal timers, and the risk of losing carried gear.

### 2.2 Physical inventory

Items should feel like objects, not abstract stats. Guns use magazines, magazines contain ammo, medkits have durability, and containers reveal items through searching.

### 2.3 Tactical visibility

The player should not have perfect information. Buildings block vision, outside players cannot see stairs/interiors, and indoor players only see inside the current building.

### 2.4 Browser-first simplicity

The game should remain playable as a browser game with no backend requirement. Current architecture is a single HTML file with canvas rendering, JavaScript game state, generated item sprites, and procedural audio tones.

## 3. Current technical architecture

The game currently lives primarily in:

- `app/index.html`

The implementation is intentionally simple:

- HTML panels for menus, stash, traders, inventory, loot, inspect, health, and raid setup.
- CSS embedded in the HTML.
- JavaScript embedded in the HTML.
- Canvas rendering for raid gameplay.
- In-memory arrays/objects for player state, stash, inventory, equipment, traders, loot, Scavs, buildings, doors, stairs, and map props.
- Runtime-generated item icons using small canvas sprites.
- Procedural WebAudio tones for simple shooting/reload/loot/stair/hit sounds.

Current persistence state:

- Stash, cash, trader rep, completed tasks, equipment, and inventory are in-memory only.
- Refreshing the page resets progression.
- Persistence is not yet solved; see ADRs.

## 4. Current game modes / flow

### 4.1 Main menu

The main menu is the hub. It gives access to:

- Start Raid
- Stash
- Traders
- Character equipment / inventory indirectly through stash and raid inventory
- Cash/resources

The intended vibe is dark tactical extraction-shooter staging, not arcade brightness.

### 4.2 Raid setup

The raid setup currently supports one map:

- Ezra Town
  - Raid location subtitle: Streets of Browz

Time options:

- Day Raid
- Night Raid

Night raids use flashlight visibility. Day raids skip the darkness/flashlight overlay.

The map has six randomized spawn points. A Blacksite-side spawn was intentionally removed.

Current spawn names:

- Canal Underpass
- Old Pawn Alley
- Market Bus Stop
- Depot Wall
- Canal Flats
- South Service Road

## 5. Map design

### 5.1 Current map

The playable raid map is Ezra Town / Streets of Browz.

It contains:

- Apartment buildings
- Market block
- Municipal offices
- Transit depot
- Canal flats
- Pawn block
- Hotel
- Clinic
- Old bank
- Metro offices
- South tenements
- Cinema
- Parking structure
- Blacksite Annex
- Roads and alleys
- Trees and bushes
- Gardens
- Small structures
- A hidden crew hideout shell

Important decision:

- The hideout area exists as a place for future boss content.
- No boss should be added yet.

### 5.2 Buildings

Buildings have physical walls and doors. The player should enter through doors rather than walking through walls.

Buildings may contain:

- Staircases
- Desks / ground loot
- Loot containers
- Multi-floor loot
- Scav entry/chase routes

### 5.3 Floors and staircases

The player can use stairs to go upstairs/downstairs.

Current floor rules:

- When upstairs, the player is confined to the building.
- The player cannot walk outside while still on floor 2.
- Stairs are hidden from outside view.
- Scavs can route to staircases and climb to the player’s floor.

### 5.4 Visibility / occlusion

Visibility is intentionally restricted:

- Outside buildings:
  - Buildings render as solid roofs.
  - Stairs and interior loot are hidden.
- Inside a building:
  - Only the current building interior is visible.
  - The outside world is blacked out.
- Upstairs:
  - Only the current building/floor context should be visible.

This prevents the player from using top-down x-ray vision to identify stairs, loot, or threats inside unopened buildings.

## 6. Day/night and lighting

### 6.1 Day raids

Day raids use normal visibility. They are intended to be clearer and easier to read, but Scavs should plausibly spot the player more easily in later balancing.

### 6.2 Night raids

Night raids use a flashlight cone from the player’s weapon.

Current night-lighting decisions:

- The world is dark outside the cone.
- The flashlight cone uses raycasting.
- Buildings block flashlight rays.
- The cone redraws actual world content inside the lit area.
- Flashlight edge artifacts should be minimized with high ray count and wall-hit refinement.

Known future tuning:

- The flashlight edge still needs visual QA whenever building collision changes.
- More atmospheric lighting can be added later, but should not bring back x-ray visibility.

## 7. Player controls

Current controls:

- `WASD`: Move
- Mouse: Aim
- Left mouse: Shoot / hold to fire
- `E`: Interact / loot / stairs / doors
- `F`: Quick loot when inside a container and hovering a revealed item
- `I`: Inventory
- `H`: Health panel
- `R`: Reload
- `2`: Primary weapon
- `3`: Secondary weapon
- `1`, `4`-`0`: Hotbar items

Hotbar decisions:

- Primary weapon is always `2`.
- Secondary weapon is always `3`.
- Inventory items can be bound to other hotbar keys by hovering and pressing a number while inventory is open.
- Item icons show the assigned hotbar number.

## 8. Combat

### 8.1 Weapons

Current weapons:

- Pistol
- M4
- Sniper

Each weapon has:

- Damage
- Fire rate
- Bullet speed
- Magazine capacity
- Ammo type
- Required magazine type

### 8.2 Magazine system

The game uses actual magazine items.

Current magazines:

- `Pistol9Mag`
  - Gun: Pistol
  - Ammo: 9mm
  - Capacity: 12
- `Gen3M4Mag`
  - Gun: M4
  - Ammo: 5.56
  - Capacity: 30
- `Sniper762Mag`
  - Gun: Sniper
  - Ammo: 7.62
  - Capacity: 5

Important decisions:

- Reloading swaps a loaded inventory magazine into the weapon.
- The ejected magazine returns to inventory with its remaining ammo preserved.
- Empty ejected magazines are preserved too.
- Dragging correct ammo onto a magazine loads it round-by-round.
- Wrong ammo is rejected.

### 8.3 Reloading

Reloading is timed.

Current decision:

- Reloading takes 3 seconds.
- The player cannot shoot while reloading.
- A progress overlay is shown.
- Reload completion swaps magazines.

### 8.4 Bullets and walls

Bullets are visible and should be readable during day and night raids.

Important decisions:

- Player bullets collide with buildings.
- Enemy bullets collide with buildings.
- Scavs should only shoot if they have line of sight.
- Shooting through buildings should not be possible.

### 8.5 Muzzle flash

Current decision:

- Muzzle flash is short and flickers per shot.
- Holding fire should not make a static flash.
- Bullets should spawn while holding fire, not after releasing the mouse button.

## 9. Scav AI

Scavs are hostile AI enemies.

Current behavior:

- Spawn around the raid map.
- Patrol/chase based on proximity and recent gunshots.
- Hear nearby gunshots.
- Move faster than earlier versions.
- Route toward doors if the player is inside.
- Enter the player’s building.
- Use stairs to chase the player upstairs/downstairs.
- Respect floors when attacking and being attacked.
- Drop a lootable corpse on death.

Current animation:

- Basic walking bob/leg movement.

Known future work:

- AI pathfinding is still simple steering plus door/stair routing.
- A full navmesh/grid pathfinder is not implemented.
- See ADRs.

## 10. Looting

### 10.1 Loot sources

Current loot sources:

- Wooden crates
- Supply containers
- Metal lockers/safes
- Military cases
- High-tech / Blacksite containers
- Ground loot
- Desk loot
- Dead Scav bodies

### 10.2 Container searching

Containers use Tarkov-style searching:

- Items are hidden as unknown slots when the container opens.
- Items reveal sequentially.
- Each item has a locked search offset/duration when the container opens.
- Quick looting a revealed item does not speed up searching of later items.

### 10.3 Quick loot

Current quick-loot decision:

- Outside-container quick-loot UI prompt is removed.
- Inside a container, hover a revealed item and press `F`.
- The hovered revealed item moves directly to inventory.
- Hidden items cannot be quick-looted.

### 10.4 Ground / desk loot

Desk loot exists inside buildings and may spawn items such as:

- Bolts
- Screws
- Wires
- Phones
- Flash drives
- Blacksite keycards

Desk loot is floor/building-aware:

- It only appears when the player is on the correct floor/building.

### 10.5 Blacksite loot

The Blacksite Annex contains high-tier loot.

Current decision:

- A special Blacksite vault crate exists.
- It rolls extra high-tier loot.
- Blacksite keycards can spawn from desk/rare loot.

## 11. Item system

Items have:

- Name
- Quantity
- Value
- Generated sprite/icon
- Description
- Optional ammo count
- Optional durability

Item examples:

- Ammo: `Ammo9`, `Ammo556`, `Ammo762`
- Magazines: `Pistol9Mag`, `Gen3M4Mag`, `Sniper762Mag`
- Medical: `Bandage`, `Medkit`, `SurgicalKit`, `Painkillers`
- Technical loot: `Bolts`, `Screws`, `Wires`, `Battery`, `CPU`, `GPU`, `AdvancedGPU`, `SSD`, `MilitarySSD`
- Valuables: `FlashDrive`, `Phone`, `GoldWatch`, `Jewelry`
- Access: `Key`, `Keycard`
- Gear: `Armor`, `Helmet`, `Backpack`
- Weapons: `Pistol`, `M4`, `Sniper`

## 12. Health and medical system

The game uses Tarkov-inspired body parts.

Body parts:

- Head
- Thorax
- Left arm
- Right arm
- Left leg
- Right leg

Each part has:

- HP
- Max HP
- Bleeding flag
- Blacked flag

### 12.1 Bandages

Bandages stop bleeding.

Current decision:

- Bandaging is timed.
- Bandages are consumed on use.

### 12.2 Medkits

Medkits heal damaged body parts.

Current decisions:

- Medkits have durability.
- Medkits heal over time.
- Medkits cannot repair blacked body parts.
- If a body part is blacked, the player needs a Surgical Kit before medkit healing can fully matter.

### 12.3 Surgical Kits

Surgical Kits repair blacked non-lethal body parts.

Current decisions:

- Surgery takes 10 seconds.
- Surgery restores one blacked limb to 1 HP.
- Surgical Kits are consumed on use.
- Head/thorax blacking remains lethal.

## 13. Inventory and stash

The inventory system is intentionally Tarkov-inspired.

Current systems:

- Character equipment slots:
  - Primary weapon
  - Secondary weapon
  - Armor
  - Helmet
  - Backpack
- Backpack inventory
- Stash inventory
- Raid inventory
- Drag-and-drop item movement
- Drag ammo onto magazines to load them
- Double-click item inspection
- Hotbar item binding

Important decision:

- The same inventory/equipment setup is used in stash and raid.
- Starting a raid uses the prepared equipment/backpack.
- If the raid backpack is empty but stash has useful deployment supplies, the game may auto-pack compatible mags/ammo/meds to avoid spawning with only a gun.

## 14. Inspect window

Double-clicking an item opens an inspect window.

Current inspect window shows:

- Icon
- Item name
- Item type
- Description
- Stats such as value, stack, loaded ammo, magazine type, durability, or use time

The inspect window is draggable by its header.

## 15. Traders

Traders currently exist as a hub system.

Current trader structure:

- Multiple traders with tabs/portraits
- Buy/sell market list
- Selectable item detail/deal panel
- Buy button
- Sell button
- Trader reputation
- Tasks/turn-ins

Trader tasks include finding and turning in items such as:

- Bolts
- Screws
- Wires
- Bandages
- Batteries
- Flash drives
- Jewelry
- Magazines
- GPUs

Current traders:

- Scavenger
- Field Medic
- Quartermaster
- Mechanic
- Broker

## 16. UI direction

The UI should feel:

- Dark
- Tactical
- Military
- Dense but readable
- Tarkov-inspired without copying exact UI

Important UI elements:

- Main menu / hub
- Raid setup
- Stash
- Traders
- Raid inventory
- Loot container panel
- Inspect item panel
- Health panel
- Top/right health body HUD
- Hotbar
- Reload/healing/progress overlay

## 17. Art direction

Current art is canvas-generated, not external sprite sheets.

Style:

- Top-down tactical bodies
- Simple guns
- Generated item icons
- Dark city map
- Solid building roofs when outside
- Interior reveal when inside
- Bright flashlight for night raids

Future art can move to sprite sheets if the project outgrows canvas-generated sprites.

## 18. Audio direction

Current audio is procedural WebAudio:

- Gunshot
- Reload
- Loot/open
- Stairs
- Hit

No external sound asset pipeline exists yet.

## 19. Current testing approach

Testing has been done through local browser automation and syntax checks.

Useful tested behaviors:

- Script syntax
- Starting raids
- Day/night selection
- Firing while holding mouse
- Reload timing
- Magazine preservation
- Container search sequencing
- Quick-loot not speeding search
- Surgical Kit timer
- Building/floor containment
- Scav stair chasing
- Expanded map/loot generation

## 20. Current repo workflow decision

User instruction:

- Each completed chunk of work should be automatically committed locally.
- Commits should be made on `main` unless the user explicitly asks for a different branch.
- Do not push automatically.
- The user will push manually unless they explicitly ask Codex to push.

Recent committed state:

- Commit `8832b8b Expand raid gameplay systems` was pushed to `main`.

Documentation and implementation chunks should be committed locally after completion, but not pushed automatically.

## 21. Project memory / checkpoint workflow

The project uses a docs-as-memory workflow so future Codex sessions can resume without the full chat history.

Canonical memory docs:

- `docs/PROJECT_STATE.md`
- `docs/PROJECT_MEMORY.md`
- `docs/SESSION_HANDOFF.md`
- `docs/GAME_DESIGN.md`
- `docs/prd/`
- `docs/adr/`

User command:

- `/checkpoint`

When the user enters `/checkpoint`, treat it as the final command before a new session. Update the current state file, session handoff, GDD, PRDs, and ADRs so the next agent can reproduce the current design and implementation direction.

Important decision:

- `/checkpoint` means update the project memory files and commit that checkpoint locally.
- `/checkpoint` does not mean push.
- Push only happens when the user explicitly asks Codex to push.

Reference:

- `docs/PROJECT_MEMORY.md`
- `docs/adr/ADR-0009-context-memory-and-checkpoint-system.md`
