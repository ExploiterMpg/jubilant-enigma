# ADR-0007 — Map Expansion and Verticality

Status: Proposed

## Context

Streets of Browz now includes:

- Roads.
- Large buildings.
- Multi-floor staircases.
- Gardens.
- Small structures.
- A hideout shell.
- Blacksite Annex.
- Desk loot.
- More container types.

The map is still hand-authored in JavaScript arrays.

## Decision needed

Decide how complex verticality should become.

## Open questions

- Should every building have multiple floors?
- Should rooftops be separate from second floors?
- Should windows matter for shooting/visibility?
- Should upstairs containers be riskier/better?
- Should Scavs spawn indoors?

## Recommendation

Keep verticality readable:

- Use floor 0 for street/interior ground floor.
- Use floor 1 for upstairs/roof gameplay.
- Only add more floors if the UI clearly communicates them.

Add indoor Scav spawns gradually after AI pathing is more reliable.

