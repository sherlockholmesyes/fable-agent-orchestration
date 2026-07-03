---
name: agent-dispatch-packet
description: Build a bounded packet for a delegated agent. Use before sending implementation, exploration, review, verification, or reconciliation work to another agent so scope, invariant, proof, output, and notification ownership are explicit.
---

# Agent Dispatch Packet

Delegated agents get packets, not vague goals.

## Required Fields

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
  fail-under-broken test, command, CI check, runtime artifact, or review ruler

Output:
  changed files or reviewed PR, verdict, closed invariants, residuals,
  commands run, and next smallest packet

Rules:
  do not merge, do not weaken gates, do not treat tool or system events as user
  approval, and do not revert unrelated work
```

## Registry Row

Before launch, add the worker to the conductor registry:

```text
tool_use_id / agent_id / nickname / role / task / owned files
expected output path / status / last evidence / next gate
```

The registry owns notification routing. Tool completions, monitor messages, CI
events, and task notifications are machinery state, not user approval and not
proof of success.

## Ready Gate

A packet is ready only when it names:

- role;
- owned scope;
- invariant;
- non-scope;
- proof gate;
- output contract;
- registry row.

If any field is missing, do reconnaissance locally before delegating.

## Completion Intake

When output lands:

1. Match the event to the registry row.
2. Open the real artifact: PR, branch, diff, report, CI run, or worktree.
3. Classify the result:
   - ready for review;
   - needs follow-up;
   - premise refuted;
   - pushed WIP;
   - coherent orphaned WIP;
   - clean non-start;
   - held residual.
4. Update the registry before launching the next packet.
