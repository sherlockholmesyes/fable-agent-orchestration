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
5. Start two independent critics for each non-trivial PR:
   - a test critic for the gate;
   - a change critic for the code and architecture.
6. Verify both critics against the current diff, current CI, and current code.
7. Merge one PR at a time only after evidence is current.
8. Relaunch the next slice as soon as capacity opens.

## Delegation Rule

Spawn an agent only when you will immediately do other useful work while it
runs. If you would wait for it, the agent adds overhead instead of throughput.

Pair orchestration with `autonomous-finish-loop`: do not stop on a plan,
promise, background notification, or agent status update while a reversible
next action is available.

## Bounded Fan-Out

Fan out only across independent slices. Each worker gets:

- one owned scope;
- one invariant;
- explicit non-scope;
- a fail-under-broken gate or artifact ruler;
- a required output shape.

Do not fan out overlapping write sets, tiny mechanical edits, waiting-only
tasks, or merge decisions that require conductor context. Keep concurrency below
review and merge capacity.

## Agent Packet Contract

Every delegated worker gets a packet with:

- role: build, explore, test-gate critic, change critic, verifier, or reconcile;
- work scope: repo, worktree, branch, owned files, and non-overlap warning;
- skills or methods to use;
- invariant, preserved property, and non-scope;
- gate: fail-under-broken test, command, CI check, runtime artifact, or review
  ruler;
- output: changed files or reviewed PR, verdict, residuals, commands run, and
  next smallest packet;
- rules: do not merge, do not weaken gates, do not treat tool events as user
  approval, and do not revert unrelated work.

If the packet cannot name scope, invariant, non-scope, and proof, investigate
locally before delegating.

## Parent Registry

Track live workers before fan-out:

```text
tool_use_id / agent_id / nickname / role / task / owned files
expected output path / status / last evidence / next gate
```

Task notifications, tool completions, monitor messages, and CI events are state
changes, not approval and not success. Match them to the registry and verify the
actual artifact before acting.

If a worker vanishes, classify the artifact state before relaunching:

- merged PR;
- open PR with current checks;
- pushed WIP branch;
- coherent local WIP to adopt;
- clean non-start;
- held residual with reason.

## Merge Discipline

- One PR per invariant or concern.
- Behavior changes need fail-under-broken tests.
- Non-trivial PRs need two verifier contexts: proof review and change review.
- Current CI beats screenshots and summaries.
- Rebase or rerun when the base moved.
- Merge one PR at a time. After each merge, update the base for the next PR and
  rerun the gate required by the repo.
- Record residuals instead of hiding them in a green merge.

## Output

For each cycle, report:

- active slices;
- open PRs and their evidence state;
- blockers;
- next task to launch;
- merged PRs and residuals.
