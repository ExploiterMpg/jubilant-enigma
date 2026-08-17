# ADR-0002 — AI Pathfinding and Building Navigation

Status: Proposed

## Context

Current Scav AI uses direct steering plus special handling for:

- Hearing gunshots.
- Routing to doors.
- Entering buildings.
- Routing to staircases.
- Switching floors.
- Line-of-sight before shooting.

This is good enough for early gameplay but can fail on complex map geometry.

## Decision needed

Decide whether to keep steering AI or move to explicit pathfinding.

## Options

### Option A — Keep steering plus hand-authored door/stair targets

Pros:

- Simple.
- Easy to tune.
- Works with current map size.

Cons:

- Can get stuck on buildings/props.
- Harder to support more complex interiors.

### Option B — Grid-based A*

Pros:

- Robust navigation around buildings.
- Can support indoors/outdoors/floors with different grids.

Cons:

- More code.
- Needs collision-grid generation.
- Needs performance care.

### Option C — Hand-authored nav nodes

Pros:

- More authored tactical flow.
- Good for a fixed map like Streets of Browz.

Cons:

- More manual data.
- Can become tedious as the map grows.

## Recommendation

Use a hybrid:

- Short term: keep steering plus door/stair routing.
- Medium term: add nav nodes for streets, building entrances, staircases, and key interiors.
- Long term: consider A* only if nav-node routing is not enough.

## Follow-up work

- Add nav points near each road intersection.
- Link nav points to building doors.
- Link doors to interior/stair nodes.
- Let Scavs path to the player’s last heard/seen node.

