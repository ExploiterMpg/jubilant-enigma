# ADR-0008 — Audio Asset Pipeline

Status: Proposed

## Context

Current audio uses procedural WebAudio tones for:

- Gunshots.
- Reload.
- Loot.
- Stairs.
- Hits.

This avoids external assets but sounds synthetic.

## Decision needed

Decide whether to keep procedural audio or add sound assets.

## Options

### Option A — Procedural only

Pros:

- No assets.
- Small game size.
- Easy to tune in code.

Cons:

- Less realistic.
- Harder to make satisfying gunshots/footsteps.

### Option B — Add local audio files

Pros:

- Better game feel.
- More recognizable feedback.

Cons:

- Requires asset sourcing/licensing.
- More files to manage.

## Recommendation

Use procedural audio for now.

When the core loop is stable, add licensed/created sounds for:

- M4 shot.
- Pistol shot.
- Sniper shot.
- Dry fire.
- Mag out/in.
- Footsteps.
- Container search.
- Scav bark/hit/death.

