---
name: one-slice-worker-cycle
description: Execute one correctness-critical slice through investigation, narrow implementation, and falsification proof. Use for hot-path, security, durability, recovery, concurrency, runtime, or product-surface changes where a happy-path patch is not enough.
---

# One Slice Worker Cycle

Use this cycle for one delicate task.

## Phase 1: Investigate

Do not build until the real hazards are named or the diagnosis is refuted.

- Read current code at HEAD.
- Map all readers and writers of touched state.
- List concurrency, recovery, and lifecycle edges.
- Identify what the task packet does not mention.
- If the diagnosis is wrong, stop and report refutation.

## Phase 2: Build Narrow

Do not bundle adjacent refactors.

- Keep the diff scoped to one rule.
- Preserve existing call-site behavior unless changing it is the task.
- Prefer structural guarantees over remembered flags.
- Put state in data structures, not in process rituals.

## Phase 3: Prove

Do not call the slice complete until evidence discriminates broken from fixed.

- Add one fail-under-broken gate per hazard.
- Exercise production functions, not a reimplementation inside the test.
- Run a negative-control or sabotage variant when practical.
- Run the relevant full gate.
- Send the PR to independent review.

## Result States

- `DONE`: fixed, tested, reviewed, ready for merge.
- `HELD`: plausible but cannot be proven safely yet.
- `REFUTED`: original diagnosis was wrong.
- `BLOCKED`: external input or environment is required.
