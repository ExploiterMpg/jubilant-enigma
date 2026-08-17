# ADR-0001 — Persistence and Save System

Status: Proposed

## Context

The current game stores stash, inventory, cash, trader reputation, trader task completion, equipment, health, and raid state in memory.

Refreshing the browser resets progress.

The game’s long-term extraction loop depends on persistence, but persistence has not been implemented yet.

## Decision needed

Choose how progression should be saved.

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

Start with LocalStorage plus optional JSON export/import.

Do not add a backend until the single-player loop is stable.

## Follow-up work

- Define a `profileVersion`.
- Save stash, inventory, equipment, cash, trader rep/tasks, and settings.
- Add migration helpers.
- Add reset profile button.
- Add export/import buttons.

