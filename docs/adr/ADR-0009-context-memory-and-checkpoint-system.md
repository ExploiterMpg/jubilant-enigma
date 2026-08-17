# ADR-0009 — Context Memory and Checkpoint System

Status: Accepted  
Last updated: 2026-08-16

## Context

Extraction Zone is being built through iterative Codex sessions. A fresh session may not have the full chat history, but the project contains many design and implementation decisions that must remain consistent.

The user wants a simple command:

```text
/checkpoint
```

When entered, it should save the session state so a fresh agent can resume.

## Decision

Use a docs-as-memory system inside the repo.

The canonical memory stack is:

- `docs/PROJECT_STATE.md`
- `docs/SESSION_HANDOFF.md`
- `docs/GAME_DESIGN.md`
- `docs/prd/`
- `docs/adr/`

The `/checkpoint` command is a project protocol, not a game runtime feature and not a custom shell command.

When the user enters `/checkpoint`, the agent updates:

- the current-state snapshot,
- the session handoff,
- settled game design decisions,
- PRDs for user-facing requirements,
- ADRs for architecture, algorithm, implementation, and unresolved decisions.

The command implies a local commit after the memory files are updated, but it does not imply a push.

## Why this approach

Pros:

- Simple.
- Works without a backend.
- Works across fresh Codex sessions.
- Easy for humans to read.
- Versionable in Git.
- Keeps design decisions near the code.

Cons:

- Requires agents to follow the protocol.
- Markdown state can drift if not updated.
- Not automatically enforced by tooling.

## Alternatives considered

### Single giant handoff document

Rejected as the only system because it becomes too long and makes architectural decisions hard to find.

### JSON-only state file

Rejected for now because human readability matters more than machine parsing.

### External memory service

Rejected for now because it introduces accounts, hosting, and unnecessary complexity.

## Implementation details

`docs/PROJECT_STATE.md` should stay concise and represent the latest resumable state.

`docs/GAME_DESIGN.md` should contain settled player-facing game design.

`docs/prd/` should contain product requirements and acceptance criteria.

`docs/adr/` should contain decisions, tradeoffs, proposed work, and deferred architecture/algorithm choices.

Unfinished topics should not disappear. They should be captured as:

- `Proposed`, if likely but undecided.
- `Deferred`, if intentionally postponed.
- `Accepted`, if decided and expected to guide implementation.
- `Superseded`, if replaced.

## `/checkpoint` required steps

1. Inspect repo status.
2. Identify changed code/docs and any new decisions made in the chat.
3. Update `PROJECT_STATE.md`.
4. Update `SESSION_HANDOFF.md`.
5. Update `GAME_DESIGN.md` for settled design.
6. Update or add PRDs for feature requirements.
7. Update or add ADRs for implementation/architecture decisions.
8. Run lightweight validation when practical.
9. Report updated files and git status.

## Git workflow interaction

The user’s standing rule remains:

- Each completed chunk of work should be automatically committed locally.
- Commit to `main` unless the user explicitly asks for a different branch.
- After implementation changes, run a mandatory code review by a sub-agent before committing the chunk.
- Do not push automatically.
- The user will push manually unless they explicitly ask Codex to push.

Therefore `/checkpoint` saves memory files and commits them locally as the final handoff chunk, but does not push.

## Future optional improvement

Add a small script such as `scripts/checkpoint-summary.js` later to print:

- git status,
- latest commit,
- modified docs,
- recommended next reads.

Do not add this script until useful; the current Markdown protocol is enough.
