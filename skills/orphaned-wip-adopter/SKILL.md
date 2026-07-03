---
name: orphaned-wip-adopter
description: Salvage coherent uncommitted work left by a stalled worker session. Use when a parallel worktree contains valuable WIP and you need to decide whether to adopt, finish, rebase, and ship instead of rebuilding from scratch.
---

# Orphaned WIP Adopter

Adopt stalled work only after confirming it is not active.

## Procedure

1. Confirm the worktree is stale:
   - check file modification times;
   - check running processes or session status when available;
   - avoid touching active work.
2. Read the uncommitted diff.
3. Assess coherence:
   - does it compile or plausibly compile?
   - does it use existing APIs correctly?
   - is the design sound?
4. Exercise the load-bearing claim.
5. Rebase or rebuild on current main.
6. Fill gaps, add fail-under-broken proof, and review.
7. Credit the adopted work neutrally in the PR body if appropriate.

## Decision

- Adopt when coherent and faster than rebuilding.
- Rebuild when the WIP is incoherent or unsalvageable.
- Wait when the session may still be active.
