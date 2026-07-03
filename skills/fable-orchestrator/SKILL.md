---
name: fable-orchestrator
description: Coordinate a large multi-PR coding engagement by launching build agents in isolated git worktrees, validating each pull request independently, serializing merges, and keeping bounded parallel flow. Use when a backlog is too large for one linear coding pass.
---

# Fable Orchestrator

Act as the conductor for many implementation slices.

## Loop

1. Split work into independent slices.
2. Start each slice from the default branch in its own git worktree.
3. Give each agent one invariant or product concern, explicit non-scope, and
   required proof.
4. Require a pull request, not a direct merge.
5. Start an independent review for each PR.
6. Verify the review against the current diff, current CI, and current code.
7. Merge one PR at a time only after evidence is current.
8. Relaunch the next slice as soon as capacity opens.

## Delegation Rule

Spawn an agent only when you will immediately do other useful work while it
runs. If you would wait for it, the agent adds overhead instead of throughput.

## Merge Discipline

- One PR per invariant or concern.
- Behavior changes need fail-under-broken tests.
- Current CI beats screenshots and summaries.
- Rebase or rerun when the base moved.
- Record residuals instead of hiding them in a green merge.

## Output

For each cycle, report:

- active slices;
- open PRs and their evidence state;
- blockers;
- next task to launch;
- merged PRs and residuals.
