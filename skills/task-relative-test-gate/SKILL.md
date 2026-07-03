---
name: task-relative-test-gate
description: Design or review tests so they prove the specific task, not merely that generic checks are green. Use after non-trivial fixes, runtime demos, CI repairs, agent PRs, and any claim that a feature works.
---

# Task Relative Test Gate

Generic green tests are not proof. A task is verified only when the gate
exercises the task trajectory and fails under the broken behavior.

## Procedure

1. State the task trajectory: input, path, state transition, observable output.
2. Name the invariant.
3. Name explicit non-scope.
4. Name the easy fake pass.
5. Build the smallest gate that exercises the real production path.
6. Run both arms when practical:
   - broken arm fails;
   - fixed arm passes.
7. Preserve evidence: command, log, artifact, screenshot, report, or CI URL.

## Reality Gates

For product or game claims, prefer at least two truth sources:

- browser pixels or client stream;
- server state or inspector;
- worker logs;
- replay, snapshot, or durable artifact;
- load or latency report.

## Verdicts

- `PASS`: task path exercised; broken arm and fixed arm exist.
- `FAIL`: task path fails or the ruler is fake.
- `SPLIT`: useful partial proof; another truth source remains.
- `RETREAT`: this cannot be verified by the proposed gate.
