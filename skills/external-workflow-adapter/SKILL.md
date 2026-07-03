---
name: external-workflow-adapter
description: Adapt a third-party agent workflow, skill marketplace pattern, or generic plan/execute/review method into an invariant-gated local workflow. Use before importing external agent methodology so useful primitives survive without adopting hidden assumptions.
---

# External Workflow Adapter

Use external workflows as scaffolding, not law.

## Procedure

1. Name the source workflow.
2. Extract its primitives:
   - primitive;
   - value;
   - hidden assumption;
   - risk if used unchanged.
3. Decide for each primitive:
   - import as-is;
   - adapt;
   - reject.
4. Map imported primitives to local gates:
   - diff and CI for code;
   - task-relative tests for behavior;
   - runtime evidence for product claims;
   - review packets for load-bearing decisions.
5. Reject any primitive that cannot name its proof.

## Common Imports

- skill discovery;
- planning discipline;
- subagent review;
- branch finish hygiene;
- test behavior evaluation.

## Common Rewrites

- generic unit-test green becomes task-relative proof;
- broad "agent solved it" becomes claim-to-diff validation;
- "use this plugin" becomes OPSEC and authority review first;
- "ship when done" becomes serialized merge on current evidence.
