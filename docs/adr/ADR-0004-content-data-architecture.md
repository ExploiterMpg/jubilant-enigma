# ADR-0004 — Content Data Architecture

Status: Proposed

## Context

Current content data lives inside `index.html`, including:

- Guns.
- Magazines.
- Items.
- Item values.
- Item descriptions.
- Loot tables.
- Buildings.
- Doors.
- Stairs.
- Traders.
- Trader tasks.

This is fast for early development but can become difficult to maintain as the game grows.

## Decision needed

Decide whether content should remain embedded or be split into structured data files.

## Options

### Option A — Keep everything in `index.html`

Pros:

- Simple.
- Easy to ship as one file.
- No build step.

Cons:

- Large file.
- Hard to organize.
- Higher risk of accidental regressions.

### Option B — Split into JavaScript modules

Pros:

- Better organization.
- Still simple.
- No backend needed.

Cons:

- Requires local server or module-friendly hosting.
- Slightly more project structure.

### Option C — External JSON data files

Pros:

- Easier content editing.
- Potentially designer-friendly.

Cons:

- Requires fetch/loading.
- Needs validation.
- More failure cases.

## Recommendation

Move to JavaScript modules once the current feature set stabilizes:

- `data/items.js`
- `data/lootTables.js`
- `data/map.js`
- `data/traders.js`
- `systems/inventory.js`
- `systems/combat.js`
- `systems/ai.js`
- `systems/render.js`

Avoid JSON until a validation layer exists.

