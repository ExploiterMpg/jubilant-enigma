# Extraction Zone — Project State

Last checkpoint: 2026-08-16  
Checkpoint command: `/checkpoint`  
Repo: `https://github.com/ExploiterMpg/jubilant-enigma`  
Local project path: `C:\Users\aoekk\Documents\Codex\2026-08-16\https-github-com-exploitermpg-jubilant-enigma\app`  
Main game file: `app/index.html`

## Read this first

This is the short-form state file for fresh Codex sessions. It should be updated whenever the user enters `/checkpoint`.

For full context, read next:

1. `docs/SESSION_HANDOFF.md`
2. `docs/GAME_DESIGN.md`
3. Relevant files in `docs/prd/`
4. Relevant files in `docs/adr/`

## Current objective

Build Extraction Zone into a browser-based tactical extraction shooter with a persistent stash/raid loop, Tarkov-inspired inventory, looting, combat, map visibility, Scav AI, traders, and long-term progression.

The immediate meta-objective is now to preserve project context across Codex sessions through a simple docs-as-memory checkpoint system.

## Current repo state

Latest known pushed commit:

- `8832b8b Expand raid gameplay systems`

Known uncommitted docs at this checkpoint:

- `docs/GAME_DESIGN.md`
- `docs/SESSION_HANDOFF.md`
- `docs/PROJECT_MEMORY.md`
- `docs/PROJECT_STATE.md`
- `docs/prd/README.md`
- `docs/prd/PRD-0001-core-extraction-zone.md`
- `docs/adr/README.md`
- `docs/adr/ADR-0001-persistence-and-save-system.md`
- `docs/adr/ADR-0002-ai-pathfinding.md`
- `docs/adr/ADR-0003-boss-hideout.md`
- `docs/adr/ADR-0004-content-data-architecture.md`
- `docs/adr/ADR-0005-economy-and-progression-balance.md`
- `docs/adr/ADR-0006-medical-system-depth.md`
- `docs/adr/ADR-0007-map-expansion-and-verticality.md`
- `docs/adr/ADR-0008-audio-asset-pipeline.md`
- `docs/adr/ADR-0009-context-memory-and-checkpoint-system.md`

Important workflow:

- Each completed chunk of work should be automatically committed locally.
- Commits should be made on `main` unless the user explicitly asks for a different branch.
- Do not push automatically.
- The user will push manually unless they explicitly ask Codex to push.
- `/checkpoint` updates memory files and should be committed locally as its own chunk when complete.

## Current implementation summary

Extraction Zone currently runs primarily from `app/index.html`.

Implemented systems include:

- Ezra Town / Streets of Browz as the single raid map.
- Day and night raid selection.
- Night flashlight cone with building-blocked raycast visibility.
- Six random player spawns, excluding Blacksite-side spawn.
- Multi-building top-down map with doors, stairs, floors, gardens, containers, desk loot, and hideout shell.
- Outside roof occlusion and indoor-only visibility masks.
- Player equipment, stash, backpack, raid inventory, hotbar, and draggable item inspection.
- Actual magazines and ammo types.
- Timed reloads and round-by-round magazine loading.
- Body-part health, bleeding, medkit durability, timed healing, and Surgical Kit usage.
- Tarkov-style sequential container search.
- Quick loot only inside opened containers.
- Scav AI that hears shots, chases, enters buildings, uses stairs, and respects walls/floors.
- Bullets blocked by buildings.
- Procedural audio.
- Traders with rep, tasks, buy/sell selection, and item turn-ins.

## Settled design decisions

- The game is a browser-first single-player extraction shooter.
- The current game can stay single-file until feature pressure makes modularization worth it.
- Ezra Town / Streets of Browz is the only active map for now.
- The hideout shell exists, but no boss should be added until the user explicitly asks.
- Night raids require a bright weapon flashlight cone; buildings must not become x-ray visible.
- Outside players see buildings as solid roofs.
- Inside players only see the current building/floor context.
- Primary weapon is always hotbar key `2`; secondary is always key `3`.
- Magazines are real items and preserve ammo when ejected.
- Reloading takes 3 seconds.
- Surgical Kits take 10 seconds and repair blacked non-lethal limbs.
- Quick loot applies only to revealed items inside an opened container.

## Open / unfinished decisions

See ADRs for details.

High-priority unfinished areas:

- Browser LocalStorage persistence and profile migration.
- More robust Scav navigation/pathfinding.
- Economy balance and trader progression.
- Medical penalties for blacked limbs.
- Data/module split once `index.html` becomes too fragile.
- Audio asset pipeline.
- Boss/hideout content later, only when explicitly requested.

## `/checkpoint` protocol

When the user enters `/checkpoint`, update:

- `docs/PROJECT_STATE.md`
- `docs/SESSION_HANDOFF.md`
- `docs/GAME_DESIGN.md` if settled design changed
- `docs/prd/*` if product requirements changed
- `docs/adr/*` if architecture/implementation/algorithm decisions changed

Then report the updated files and current git status.

Commit the checkpoint updates locally when complete. Do not push.

## Recommended next implementation task

Add the actual browser save/profile system:

- LocalStorage profile with `profileVersion`.
- Save/load stash, cash, equipment, backpack inventory, trader rep/tasks, settings, and hotbar.
- Add export/import/reset later.

Reference:

- `docs/adr/ADR-0001-persistence-and-save-system.md`
