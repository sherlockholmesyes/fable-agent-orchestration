---
name: fable
description: Coordinate a large multi-change coding engagement by running several implementation agents in isolated git worktrees, validating each pull request with independent review, and merging only on evidence. Use when the work is bigger than one patch and throughput should come from orchestration rather than hand-coding every slice.
---

# Fable orchestration playbook

Drive a large multi-slice engagement as the conductor. Agents do scoped
implementation work. The conductor owns the queue, evidence, review, and merge
gate.

The outer loop is fan-out, review, merge, and relaunch. Each worker runs the
inner loop on one slice: investigate, build narrow, prove by falsification.

## The core loop

1. Launch one build agent in its own git worktree from the default branch.
   Example: `git worktree add <path> origin/main -b codex/<slice-name>`.
2. Give the agent one invariant or product slice, explicit non-scope, and the
   required proof. The agent opens a pull request and does not merge.
3. Launch two separate critics:
   - a test-gate critic that checks whether the proof is task-relative and
     would fail under the broken behavior;
   - an adversarial change critic that reviews the code, architecture, and
     operational risk.
4. Verify both critics against the current diff, current CI, and the actual
   code at HEAD. A review can be stale or wrong.
5. Land real follow-ups in the same PR.
6. Merge one PR at a time only on current evidence.
7. Relaunch the next slice while other agents, reviews, and CI are running.

## When to spawn an agent

The axis is parallelism gain, not patch size.

Spawn a build agent when:

- the slice is self-contained;
- it can run in a separate worktree without conflict;
- launching it lets you immediately work on another slice, review, or merge.

Do it yourself when:

- the delta is smaller than the prompt;
- the task is a merge decision requiring full local context;
- investigation must be done before a safe delegation prompt can be written;
- an agent has failed twice and a manual finish is faster.

Always use independent review for load-bearing work. For non-trivial slices,
split review into two contexts: proof critic and change critic. Do not spawn
more agents than you can review and merge.

## Disciplines

- One PR per invariant or concern.
- Every behavior claim needs a fail-under-broken gate that fails on the old
  broken behavior and exercises the production path.
- Investigate before building on hot-path, security, durability, recovery, and
  concurrency slices.
- Prefer structural guarantees over opt-in flags.
- Verify reviewer claims against the current code.
- Treat refutation as useful output: if the diagnosis is wrong, open no PR and
  write the reason.
- Serialize merges, even when work is parallel.
- Capture residuals immediately in the task tracker.

## Inner worker cycle

Use three phases with hard exits:

- Investigate: leave only when the real hazards are named or the diagnosis is
  refuted. Read current code, map every reader and writer of touched state, and
  enumerate concurrency or recovery interleavings.
- Build narrow: leave only when the change is one coherent rule with a narrow
  diff. Do not bundle adjacent refactors.
- Prove: leave only when the task-relative test critic and the adversarial
  change critic are both green. Include a negative-control or sabotage check
  for critical behavior when practical.

## Tempo

The goal is continuous bounded flow:

```text
launch -> advance another slice -> review a returned PR -> merge a green one -> relaunch
```

Past your review capacity, more agents create backlog rather than throughput.

Two guards:

- Do not end on a plan when a reversible action is available.
- Do not wait for the human when a safe next step follows from the task.

## Scaling past your own agents

When the backlog exceeds your local agent pool:

1. Rank the hardest or most expensive nodes.
2. Write a queue file with one packet per task: files, invariant, acceptance,
   fail-under-broken gate, explicit non-scope, and report path.
3. Hand tasks out one at a time.
4. Watch the report folder and feed the next task when one lands.

The playbook exists to prevent hand-coding slice N while slices N+1..M sit idle.
Conduct: fan out, gate hard, merge clean, repeat.
