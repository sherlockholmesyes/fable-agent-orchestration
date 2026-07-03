---
name: agent-pr-validator
description: Validate agent-generated pull requests or merged changes against the actual diff, CI, invariant claims, and task-relative tests. Use when a coding agent, external tool, or automation produces a PR and a human or lead agent must decide whether to merge or follow up.
---

# Agent PR Validator

Treat PR descriptions and agent summaries as hypotheses.

## Procedure

1. Identify the exact PR, branch, head SHA, base branch, and merge state.
2. Extract the claim set from the PR body, commits, tests, and comments.
3. Classify each claim as fact, inference, or recommendation.
4. Map each claim to files, functions, tests, or runtime artifacts.
5. Check whether the changed tests would fail under the old broken behavior.
6. Fetch current CI, not stale screenshots.
7. Check whether the PR weakens a gate to make CI green.
8. Classify residuals as blocking or non-blocking.

## Verdicts

- `MERGE_OK`: evidence is current and the scoped invariant is closed.
- `MERGED_WITH_RESIDUALS`: already merged; residuals remain and should be filed.
- `FIX_FIRST`: direction is useful but a blocking invariant remains.
- `REVERT_OR_ROLL_FORWARD`: merged change is unsafe.

## Output

Return:

- current PR state;
- claims closed by code and tests;
- open residuals with references;
- next smallest task.
