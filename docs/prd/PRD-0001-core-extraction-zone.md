# PRD-0001 — Core Extraction Zone

Status: Accepted  
Last updated: 2026-08-16

## Summary

Extraction Zone is a lightweight browser extraction shooter. The player prepares gear, enters Ezra Town, loots buildings and containers, fights Scavs, finds valuable items, and extracts to keep what they survived with.

## Goals

- Deliver the core extraction loop:
  - Stash
  - Prepare
  - Raid
  - Search
  - Loot
  - Fight
  - Extract
  - Upgrade
- Make loot feel risky and valuable.
- Make inventory feel physical and tactical.
- Make raids readable as a browser game while preserving darkness, occlusion, and uncertainty.
- Keep implementation lightweight enough for fast iteration.

## Non-goals

- Multiplayer.
- Backend accounts.
- Multiple maps before Ezra Town is fun.
- Boss AI until the user explicitly asks for it.
- Perfect Tarkov simulation.

## Player-facing requirements

### Main menu / hub

The player can access:

- Start Raid
- Stash
- Traders
- Equipment/inventory
- Cash/resources

The UI should be dark, tactical, compact, and extraction-shooter inspired.

### Raid setup

The player chooses:

- Map: Ezra Town / Streets of Browz
- Time: Day or Night

Only one map is active for now.

### Raid map

Ezra Town should include:

- streets,
- apartments,
- market area,
- offices,
- Blacksite Annex,
- small buildings,
- gardens,
- stash/loot rooms,
- staircases,
- floor-specific interiors,
- hidden future boss hideout shell.

Buildings must be entered through doors and should not allow wall clipping.

### Visibility

Outside:

- Buildings appear as solid roofs.
- Stairs/interiors/loot inside buildings are hidden.

Inside:

- The player sees only the current building/floor context.
- Outside world is hidden.

Night:

- The weapon flashlight creates a bright cone.
- The cone illuminates actual world content.
- Buildings block flashlight rays.
- Muzzle flash briefly brightens shots.

### Inventory and equipment

The player has:

- primary weapon,
- secondary weapon,
- armor,
- helmet,
- backpack,
- backpack inventory,
- stash.

The raid should use the same prepared equipment/backpack setup from the stash.

### Hotbar

- Primary weapon is always key `2`.
- Secondary weapon is always key `3`.
- Other usable items can be bound to `1` and `4`-`0`.
- Bound items show their number on the item icon.
- When inventory is open, hotbars move to the bottom.

### Looting

Loot sources include:

- crates,
- military cases,
- metal containers,
- supply caches,
- Blacksite containers,
- desks,
- ground loot,
- dead Scavs.

Containers use sequential searching:

- hidden items reveal one by one,
- quick-looting one item must not speed up the remaining search,
- quick loot with `F` only works inside an opened container on a revealed hovered item.

### Combat

- Guns fire visible bullets.
- Holding fire spawns bullets during the hold, not after release.
- Muzzle flash flickers per shot.
- Player and Scav bullets cannot pass through buildings.
- Scavs need line of sight to shoot.

### Magazines and ammo

- Guns use specific magazine types.
- Magazines use specific ammo types.
- Reloading takes 3 seconds.
- Ejected magazines return to inventory with remaining ammo preserved.
- Empty magazines are preserved.
- Dragging matching ammo onto a magazine fills it slowly round by round.
- Wrong ammo cannot fill a magazine.

### Health

- Body parts include head, thorax, arms, and legs.
- Parts can bleed or become blacked.
- Bandages stop bleeding.
- Medkits heal over time and have durability.
- Medkits cannot repair blacked parts.
- Surgical Kits take 10 seconds and restore blacked non-lethal limbs.

### Scavs

Scavs should:

- patrol,
- hear nearby shots,
- chase the player,
- enter buildings,
- use stairs,
- respect floors,
- avoid shooting through buildings,
- leave lootable corpses.

### Traders

Traders should provide:

- buy/sell item lists,
- selected item detail panel,
- trader reputation,
- tasks requiring found items,
- rewards/progression hooks.

## Acceptance criteria

- A player can start from hub, enter a raid, loot, fight, and extract.
- Prepared inventory appears in raid.
- Loot extracted from raid can become future progression once persistence is implemented.
- Night raids are playable with flashlight visibility.
- Containers search sequentially.
- Reloading and magazine preservation behave correctly.
- Health items have timers and correct constraints.
- Scavs can threaten players inside buildings and upstairs.

## Related docs

- `docs/GAME_DESIGN.md`
- `docs/PROJECT_STATE.md`
- `docs/adr/ADR-0001-persistence-and-save-system.md`
- `docs/adr/ADR-0002-ai-pathfinding.md`
- `docs/adr/ADR-0009-context-memory-and-checkpoint-system.md`
