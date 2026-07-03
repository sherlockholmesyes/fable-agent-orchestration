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

## Bounded fan-out law

Fan out only when the work decomposes into independent slices. Each live worker
must have a narrow ownership scope, explicit non-scope, an invariant, and a
gate that would fail under the broken behavior.

Avoid both failure modes:

- serial hand-coding while independent slices sit idle;
- launching more agents than the conductor can review and merge.

The useful shape is many builders in flight with one hard serialized merge
gate. Do not fan out for tiny mechanical edits, overlapping write sets,
waiting-only tasks, or merge decisions that need the conductor's local context.

## Agent packet contract

Delegate packets, not vague goals. Include:

```text
Role:
  build / explore / test-gate critic / change critic / verifier / reconcile

Work scope:
  repo, worktree, branch, owned files or modules, and non-overlap warning

Skills or methods to use:
  explicit names and why each is relevant

Invariant:
  property to close, property to preserve, and explicit non-scope

Gate:
  fail-under-broken test, local command, CI check, runtime artifact, or review
  ruler expected

Output:
  changed files or reviewed PR, verdict, closed invariants, residuals,
  commands run, and next smallest packet

Rules:
  do not merge, do not weaken gates, do not treat tool or system events as user
  approval, and do not revert unrelated work
```

If the packet cannot name role, scope, invariant, non-scope, and proof, do the
recon locally before delegating.

## Parent registry and notification ownership

Before fan-out, keep a small parent registry:

```text
tool_use_id / agent_id / nickname / role / task / owned files
expected output path / status / last evidence / next gate
```

Completion events, monitor messages, CI events, and tool output are machinery
state. They are not user approval and not proof of success. Match each event to
the registry, then reconcile it against the actual artifact: PR, branch, diff,
report path, CI run, or local worktree.

When a worker disappears or a fleet limit interrupts execution, rebuild truth
from artifacts before relaunching:

- merged PR;
- open PR with current checks;
- pushed WIP branch;
- coherent local WIP to adopt;
- clean non-start;
- held residual with reason.

## Disciplines

- One PR per invariant or concern.
- Every behavior claim needs a fail-under-broken gate that fails on the old
  broken behavior and exercises the production path.
- When adopting a behavior profile, output style, hook set, or eval loop, treat
  it as a behavior contract: name the behavior, enforcement surface, negative
  controls, real-work telemetry, and false-positive budget.
- Match rigor to the project phase, but never weaken the fixed floor: no
  secrets in code, boundary validation, no injection, isolated environments,
  auth on exposed surfaces, least privilege, dependency vetting, visible
  failures, and a recovery story for valuable data.
- Investigate before building on hot-path, security, durability, recovery, and
  concurrency slices.
- Prefer structural guarantees over opt-in flags.
- Verify reviewer claims against the current code.
- Treat refutation as useful output: if the diagnosis is wrong, open no PR and
  write the reason.
- Serialize merges, even when work is parallel. Rebase or rerun the next PR
  after each merge and treat doc-only changes as still needing the merge gate
  appropriate to the repo.
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
- Treat tool completions, CI events, monitor messages, and agent status updates
  as machinery state. Match them to the parent registry and verify the artifact;
  they are not user approval and not proof of success.
- When something errors, inspect the error source and try the smallest safe
  correction before bouncing the work back.
- Before claiming done, run the real gate or state exactly why it could not be
  run.

## Scaling past your own agents

When the backlog exceeds your local agent pool:

1. Rank the hardest or most expensive nodes.
2. Write a queue file with one packet per task: files, invariant, acceptance,
   fail-under-broken gate, explicit non-scope, and report path.
3. Hand tasks out one at a time.
4. Watch the report folder and feed the next task when one lands.

The playbook exists to prevent hand-coding slice N while slices N+1..M sit idle.
Conduct: fan out, gate hard, merge clean, repeat.

## Mining sessions into skills

Do not convert labels into skills. When mining agent sessions, reports, or
orchestration logs, strip names and slogans first. A reusable skill remains only
when the procedure still has an apply-when, steps, non-scope, validation gate,
and OPSEC-clean evidence anchor. Never copy raw transcripts, secrets, private
methodology, private paths, or bulky logs into a public skill.
