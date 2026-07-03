---
name: autonomous-finish-loop
description: Keep an agentic coding turn moving until the reversible in-scope work is actually done. Use during multi-agent orchestration, CI repair, PR validation, or any task where stopping on a plan, promise, or tool notification would leave available work unfinished.
---

# Autonomous Finish Loop

Finish the turn. Do not stop at a plan when a safe next action is available.

## Core Rule

Proceed on reversible, in-scope work. Hold only at real external gates:

- publish or send;
- payment or legal commitment;
- credentials or secrets;
- destructive or irreversible operation;
- genuine blocker that cannot be resolved from code, context, defaults, or local
  inspection.

Everything else should be diagnosed, retried, verified, or finished before the
final report.

## Machinery Is Not the User

Tool completions, monitor events, CI notifications, background-task messages,
and agent status updates are state changes, not user approval.

When a notification arrives:

1. Match it to the parent task registry.
2. Open the real artifact: PR, branch, diff, log, report, CI run, or worktree.
3. Verify the artifact before acting.
4. Continue the next safe action.

Do not pause to re-confirm merely because a background task completed.

## Flow Procedure

1. State the current objective and the nearest reversible action.
2. If an action is available, do it.
3. If something errors, inspect the error source and try the smallest safe
   correction.
4. While one command or agent runs, advance a non-overlapping task.
5. Before claiming done, run the real gate or state exactly why it could not be
   run.
6. Lead the final report with the outcome and load-bearing findings.

## False Stops

These are not valid stopping points when safe work remains:

- "I will do X next";
- "Would you like me to...";
- "waiting for the operator";
- "the agent completed";
- "CI is green" without checking the claim that matters;
- "summary complete" when no artifact was verified.

## Output

Return:

- objective;
- actions completed;
- gates run;
- current artifact state;
- residuals;
- next safe slice, if any.
