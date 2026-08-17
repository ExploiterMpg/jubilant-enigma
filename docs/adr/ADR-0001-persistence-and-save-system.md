# ADR-0001 — Persistence and Save System

Status: Accepted / Implemented first pass

## Context

The game’s long-term extraction loop depends on persistence. Stash, raid/backpack inventory, equipment, cash, trader reputation, task completion, selected raid settings, hotbar bindings, and health state should survive browser reloads.

The first implementation pass now saves player profile/progression in browser storage. It does not yet resume a full generated raid world.

## Decision

Use browser `localStorage` as the first persistence layer.

Current key:

- `extraction-zone-profile-v1`

Current version:

- `profileVersion: 1`

Saved fields:

- cash,
- selected raid map,
- selected raid time,
- selected trader and selected market item UI state,
- stash,
- backpack/raid inventory,
- equipment,
- equipped weapon magazine state,
- hotbar assignments,
- trader reputation,
- trader task completion,
- body-part health state.

Implementation choice:

- Use `scheduleProfileSave()` as a debounced autosave layer.
- Use tolerant loading: no save, bad JSON, or wrong version falls back to the hardcoded starter profile instead of breaking startup.
- Do not persist complete mid-raid world state yet.

## Options

### Option A — LocalStorage

Save the player profile in browser `localStorage`.

Pros:

- Simple.
- No backend required.
- Works for a lightweight browser game.

Cons:

- Easy to clear/edit.
- Not portable between browsers/devices.
- Requires migration handling as item schemas change.

### Option B — Export/import save file

Allow manual save export/import as JSON.

Pros:

- Still no backend.
- Useful for debugging and sharing profiles.

Cons:

- Less seamless.
- Still editable by players.

### Option C — Backend account/profile

Use server-side storage.

Pros:

- More durable and secure.
- Enables leaderboards, cloud saves, and future multiplayer-like systems.

Cons:

- Much larger scope.
- Requires hosting, auth, API, and security decisions.

## Recommendation

Start with LocalStorage plus optional JSON export/import later.

Do not add a backend until the single-player loop is stable.

## Follow-up work

- Add migration helpers.
- Add reset profile button.
- Add export/import buttons.
- Decide whether mid-raid world resume should be supported.
- If mid-raid resume is added, persist generated containers, revealed/search state, loose ground loot, Scav state, corpse loot, player position/floor/building, raid timer, and extraction state.
