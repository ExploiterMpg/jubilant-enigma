# Extraction Zone — Project Memory System

Last updated: 2026-08-16

This file defines the lightweight context and memory-management system for Extraction Zone.

The purpose is simple: a fresh Codex session should be able to resume the project without needing the full previous chat history.

## Canonical memory files

Future agents should treat these files as the project memory stack, in this order:

1. `docs/PROJECT_STATE.md`
   - Fastest current-state snapshot.
   - Read this first in a fresh session.
   - Updated every time the user enters `/checkpoint`.

2. `docs/SESSION_HANDOFF.md`
   - Human-readable handoff from the most recent work session.
   - Includes unfinished work, likely bugs, repo state, and testing notes.

3. `docs/GAME_DESIGN.md`
   - Settled game-design document.
   - Contains product/gameplay decisions that are no longer merely ideas.

4. `docs/prd/`
   - Product requirement documents.
   - Each PRD describes what a feature/system should do from the player/product perspective.

5. `docs/adr/`
   - Architecture/design decision records.
   - Each ADR records why a meaningful implementation or architectural choice was made, proposed, or deferred.

## `/checkpoint` command contract

When the user sends exactly:

```text
/checkpoint
```

Treat it as the final command before the user starts a new Codex session.

The agent must:

1. Inspect the current repo state.
2. Summarize what changed since the last checkpoint.
3. Update `docs/PROJECT_STATE.md`.
4. Update `docs/SESSION_HANDOFF.md`.
5. Update `docs/GAME_DESIGN.md` if any settled game-design decision changed.
6. Update or create PRDs in `docs/prd/` for user-facing requirements.
7. Update or create ADRs in `docs/adr/` for architecture, implementation, data-model, algorithmic, persistence, AI, rendering, or workflow decisions.
8. Preserve undecided or unfinished topics as `Proposed` or `Deferred` ADRs rather than burying them in prose.
9. Run a lightweight syntax/smoke check when practical.
10. Report:
    - files updated,
    - current git status,
    - whether anything remains uncommitted,
    - what a fresh agent should read first.

Commit locally after completing the `/checkpoint` documentation update. Do not push.

## What belongs where

### `PROJECT_STATE.md`

Use for the current resumable state:

- active objective,
- current repo path,
- latest known commit,
- uncommitted files,
- current implementation summary,
- immediate next tasks,
- known bugs,
- testing status,
- checkpoint timestamp.

Keep it concise. This is the “load this into short-term memory” file.

### `GAME_DESIGN.md`

Use for settled game design:

- core loop,
- map decisions,
- health/combat/looting rules,
- inventory/trader behavior,
- UI/UX direction,
- player-facing mechanics.

Do not use it for long debate logs.

### PRDs

Use PRDs for feature/product requirements:

- what the feature is,
- why players need it,
- acceptance criteria,
- player-facing behavior,
- out-of-scope items.

PRDs should not over-specify internal algorithms unless the algorithm is player-visible.

### ADRs

Use ADRs for design/architecture/implementation decisions:

- state model,
- persistence format,
- rendering strategy,
- AI pathfinding strategy,
- data/module layout,
- algorithms,
- system tradeoffs,
- deferred decisions.

If a decision is not final, still record it as `Proposed` or `Deferred`.

## Decision status vocabulary

Use these statuses consistently:

- `Accepted`: decided and should be followed.
- `Proposed`: likely direction, not final.
- `Deferred`: intentionally postponed.
- `Superseded`: replaced by another decision.

## Fresh session startup checklist

A fresh Codex agent should:

1. Read `docs/PROJECT_STATE.md`.
2. Read `docs/SESSION_HANDOFF.md`.
3. Read `docs/GAME_DESIGN.md`.
4. Read relevant PRDs/ADRs for the requested task.
5. Inspect `git status --short`.
6. Expect completed chunks of work to be committed locally.
7. Never push unless the user explicitly asks to push.

## User workflow rules to preserve

- Each completed chunk of work should be automatically committed locally.
- Commits should be made on `main` unless the user explicitly asks for a different branch.
- Do not push automatically.
- The user will push manually unless they explicitly ask Codex to push.
- `/checkpoint` saves project memory and should be committed locally as its own chunk when complete.
