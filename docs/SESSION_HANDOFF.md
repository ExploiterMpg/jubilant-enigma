# Extraction Zone — Session Handoff

Last updated: 2026-08-16  
Repo: `https://github.com/ExploiterMpg/jubilant-enigma`  
Local project path used in this session: `C:\Users\aoekk\Documents\Codex\2026-08-16\https-github-com-exploitermpg-jubilant-enigma\app`  
Main game file: `app/index.html`

## Current Git state

Latest pushed commit on `main` from this session:

- `8832b8b Expand raid gameplay systems`

Current uncommitted local files:

- `app/docs/GAME_DESIGN.md`
- `app/docs/SESSION_HANDOFF.md`
- `app/docs/PROJECT_MEMORY.md`
- `app/docs/PROJECT_STATE.md`
- `app/docs/prd/README.md`
- `app/docs/prd/PRD-0001-core-extraction-zone.md`
- `app/docs/adr/README.md`
- `app/docs/adr/ADR-0001-persistence-and-save-system.md`
- `app/docs/adr/ADR-0002-ai-pathfinding.md`
- `app/docs/adr/ADR-0003-boss-hideout.md`
- `app/docs/adr/ADR-0004-content-data-architecture.md`
- `app/docs/adr/ADR-0005-economy-and-progression-balance.md`
- `app/docs/adr/ADR-0006-medical-system-depth.md`
- `app/docs/adr/ADR-0007-map-expansion-and-verticality.md`
- `app/docs/adr/ADR-0008-audio-asset-pipeline.md`
- `app/docs/adr/ADR-0009-context-memory-and-checkpoint-system.md`

Important user workflow rule:

- Each completed chunk of work should be automatically committed locally.
- Commits should be made on `main` unless the user explicitly asks for a different branch.
- Do not push automatically.
- The user will push manually unless they explicitly ask Codex to push.

## What this session accomplished

This session significantly expanded the browser extraction shooter.

Major gameplay systems now implemented:

- One raid map: Ezra Town / Streets of Browz.
- Day and night raid selection.
- Night flashlight with raycast/building blocking.
- Six randomized player spawns.
- Stash, equipment, backpack, and raid inventory.
- Actual magazines for guns.
- Timed reloads.
- Ammo loading into magazines.
- Hotbar binding.
- Health body-part system.
- Medkit durability.
- Timed healing.
- Surgical Kit for blacked limbs.
- Tarkov-style container searching.
- Sequential item reveal.
- Hovered quick-loot inside containers.
- Lootable Scav corpses.
- Traders with rep, tasks, buy/sell detail panel.
- Keycard / Blacksite access loop.
- Special Blacksite high-tier crate.
- Desk/ground loot with keycard chance.
- Building visibility/roof occlusion.
- Stair/floor system.
- Scavs that hear shots, enter buildings, and use stairs.
- Bullet collision with buildings.
- Simple walking animation.
- Procedural sounds.

## Newly added documentation

The following docs were created after the last GitHub push and are currently uncommitted:

- `docs/GAME_DESIGN.md`
  - Main settled game design document.
  - Use this as the design anchor for future sessions.

- `docs/PROJECT_MEMORY.md`
  - Defines the `/checkpoint` command protocol and docs-as-memory workflow.

- `docs/PROJECT_STATE.md`
  - Fast current-state snapshot for fresh Codex sessions.
  - Read this first in the next session.

- `docs/prd/`
  - Product requirement documents.
  - PRDs capture user-facing requirements and acceptance criteria.

- `docs/adr/`
  - Separate architecture/design decision records for unfinished or undecided systems.

This file, `docs/SESSION_HANDOFF.md`, is meant to be the quick-start handoff for the next Codex chat.

## `/checkpoint` memory workflow

The user requested a simple context/memory management system.

When the user enters:

```text
/checkpoint
```

Treat that as the final command before a fresh Codex session.

Required behavior:

1. Update `docs/PROJECT_STATE.md`.
2. Update `docs/SESSION_HANDOFF.md`.
3. Update `docs/GAME_DESIGN.md` if settled game-design decisions changed.
4. Update or create PRDs in `docs/prd/` for player-facing feature requirements.
5. Update or create ADRs in `docs/adr/` for architecture, algorithms, data models, implementation choices, or unfinished decisions.
6. Run a lightweight syntax/smoke check when practical.
7. Report updated files and current git status.

After `/checkpoint` updates are complete, commit them locally as the checkpoint chunk. Do not push.

Reference:

- `docs/PROJECT_MEMORY.md`
- `docs/PROJECT_STATE.md`
- `docs/adr/ADR-0009-context-memory-and-checkpoint-system.md`

## Known unfinished work / next-session tasks

### 1. Persistence / save system

Current state:

- Stash, cash, trader rep, completed tasks, inventory, equipment, and settings are in-memory only.
- Refreshing the page resets progress.

Recommended next step:

- Add a LocalStorage profile save with versioning.
- Save:
  - Stash
  - Inventory
  - Equipment
  - Cash
  - Trader rep
  - Trader task completion
  - Settings such as selected raid time
- Add reset/export/import later.

Reference:

- `docs/adr/ADR-0001-persistence-and-save-system.md`

### 2. AI pathfinding robustness

Current state:

- Scavs use direct steering plus door/stair routing.
- They can enter buildings and climb stairs.
- This works for the current map but is not robust pathfinding.

Risks:

- Scavs may still get stuck on some geometry.
- Scavs may fail if map complexity grows.

Recommended next step:

- Add navigation nodes for roads, doors, interiors, and stairs.
- Use node routing before considering full A*.

Reference:

- `docs/adr/ADR-0002-ai-pathfinding.md`

### 3. Visibility and building occlusion QA

Current state:

- Outside: buildings show as solid roofs.
- Inside/upstairs: outside is blacked out.
- Desk/ground loot is floor/building aware.

Needs QA:

- Check if loot, corpses, bullets, muzzle flash, and flashlight all respect the visibility mask in every edge case.
- Verify Blacksite visibility and locked door behavior after the roof/occlusion changes.

Recommended next step:

- Manually test:
  - Outside looking at each building.
  - Inside first floor.
  - Upstairs.
  - Night flashlight inside and outside.
  - Scav corpse visibility on different floors.

### 4. Medical balance

Current state:

- Medkits have durability.
- Medkits heal over time.
- Surgical Kit repairs one blacked limb after 10 seconds.
- Head/thorax blacking remains lethal.

Needs design/balance:

- Movement penalty for blacked legs.
- Aim/fire penalty for blacked arms.
- Painkiller role.
- Different medkit tiers.
- Surgery max-HP penalty or not.

Reference:

- `docs/adr/ADR-0006-medical-system-depth.md`

### 5. Economy and loot balance

Current state:

- Loot values and trader prices are rough.
- Blacksite loot is high-tier.
- Keycards can spawn from desks/rare loot.
- Traders have rep/tasks.

Needs balance:

- Keycard spawn rate.
- Blacksite payout.
- Trader unlocks by reputation.
- Weapon/armor pricing.
- Loot value pacing.

Reference:

- `docs/adr/ADR-0005-economy-and-progression-balance.md`

### 6. Data architecture cleanup

Current state:

- Everything is still in `index.html`.
- This was fine for fast iteration, but the file is now very large.

Recommended next step:

- Do not split immediately unless the user asks.
- When ready, split into modules:
  - `data/items.js`
  - `data/lootTables.js`
  - `data/map.js`
  - `data/traders.js`
  - `systems/inventory.js`
  - `systems/combat.js`
  - `systems/ai.js`
  - `systems/render.js`

Reference:

- `docs/adr/ADR-0004-content-data-architecture.md`

### 7. Boss hideout

Current state:

- A hideout shell exists.
- The user explicitly requested not to add the boss yet.

Do not add:

- Boss AI
- Boss loot
- Boss guards
- Boss spawn logic

Unless the user explicitly asks.

Reference:

- `docs/adr/ADR-0003-boss-hideout.md`

### 8. Audio quality

Current state:

- Sounds are procedural WebAudio tones.

Needs future work:

- Real or better-generated gunshot sounds.
- Footsteps.
- Container searching sounds.
- Scav voice/hit/death sounds.

Reference:

- `docs/adr/ADR-0008-audio-asset-pipeline.md`

### 9. Map polish

Current state:

- Streets of Browz has buildings, gardens, roads, small structures, hideout shell, Blacksite, desk loot, and more containers.

Needs future work:

- Better visual identity per district.
- More interior layout detail.
- Better rooftop/upstairs communication.
- More unique loot rooms.
- More environmental cover.

Reference:

- `docs/adr/ADR-0007-map-expansion-and-verticality.md`

## Known likely bugs / things to test first next session

Start the next session by running or manually checking:

1. Start raid with prepared inventory.
2. Day raid visibility.
3. Night raid flashlight against building corners.
4. Enter building, verify outside is hidden.
5. Go upstairs, verify player cannot exit building.
6. Shoot toward a wall, verify bullets stop.
7. Hold mouse fire, verify bullets spawn during hold.
8. Reload, verify 3-second timer and old mag preservation.
9. Open a container and quick-loot item 1, verify item 2 search does not speed up.
10. Damage/black a limb, use Surgical Kit, then medkit.
11. Let Scavs hear a shot while player is inside/upstairs, verify they enter/use stairs.
12. Extract with loot, verify stash transfer.

## Useful test commands from this session

Use the bundled Node runtime on this machine:

```powershell
& 'C:\Users\aoekk\.cache\codex-runtimes\codex-primary-runtime\dependencies\node\bin\node.exe' -e "const fs=require('fs'); const vm=require('vm'); const html=fs.readFileSync('app/index.html','utf8'); const match=html.match(/<script>([\s\S]*?)<\/script>/); if(!match) throw new Error('script tag not found'); new vm.Script(match[1]); console.log('script syntax ok');"
```

Existing smoke test used during this session:

```powershell
& 'C:\Users\aoekk\.cache\codex-runtimes\codex-primary-runtime\dependencies\node\bin\node.exe' work/test-game.cjs
```

Note:

- `work/test-game.cjs` is outside the repo and may not exist in a fresh checkout/session.
- If missing, create a fresh Playwright smoke test or manually test in browser.

## Git command pattern used

Because of Windows safe-directory issues, previous commands used:

```powershell
git -c safe.directory=C:/Users/aoekk/Documents/Codex/2026-08-16/https-github-com-exploitermpg-jubilant-enigma/app -C app status --short
```

When committing locally, this environment may require escalated permissions to write `.git/index.lock`.

Do not push automatically. If the user explicitly asks Codex to push, the remote may have newer commits. If push is rejected:

```powershell
git -c safe.directory=C:/Users/aoekk/Documents/Codex/2026-08-16/https-github-com-exploitermpg-jubilant-enigma/app -C app pull --rebase origin main
git -c safe.directory=C:/Users/aoekk/Documents/Codex/2026-08-16/https-github-com-exploitermpg-jubilant-enigma/app -C app push origin main
```

Only push when the user explicitly asks Codex to push.

## Recommended immediate next step

If the next chat continues development, the best next feature is:

1. Read `docs/PROJECT_STATE.md`.
2. Work in small chunks and commit each completed chunk locally.
3. Add LocalStorage persistence.
4. Then QA/fix any visibility/AI bugs caused by the newer building occlusion and map expansion.
