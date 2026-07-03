---
name: think-work-try
description: Run one delicate implementation slice through THINK, WORK, and TRY. Use when one task needs current-code investigation, a narrow structural patch, and falsification proof before a pull request can be trusted.
---

# Think Work Try

Use this as the worker loop for one non-trivial slice.

## THINK

Exit only when the real hazards are named or the diagnosis is refuted.

- Read the current code, not a remembered summary.
- Map touched state: readers, writers, owners, lifecycle, and recovery paths.
- Identify concurrency, crash, stale-state, and compatibility edges.
- If the task diagnosis is wrong, report `REFUTED` and open no PR.

## WORK

Exit only when the change is narrow and coherent.

- Build one rule, not several unrelated patches.
- Keep non-scope untouched.
- Prefer structural guarantees over opt-in switches.
- Preserve compatibility unless breaking it is the explicit task.

## TRY

Exit only when the proof would fail under the broken behavior.

- Add a task-relative fail-under-broken gate.
- Use the same production path the real system uses.
- Add a negative-control or sabotage check for critical behavior when practical.
- Run the relevant local and CI gates.
- Send the PR to the two-critic review loop.

## Output

Return one of:

- `DONE`: implementation, proof, and review are clean.
- `REFUTED`: the diagnosis was wrong.
- `HELD`: plausible but cannot be proven safely.
- `BLOCKED`: external input or environment is required.
