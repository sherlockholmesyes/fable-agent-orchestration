---
name: investigate-before-fix
description: Confirm a diagnosis against current code before building its fix. On a delicate, hot-path, security, or recovery change, a refutation — the diagnosed cause was wrong, so nothing ships — is a valid and valuable outcome. Use before committing to a fix, even outside a full slice.
---

# Investigate Before Fix

A fix for a cause you have not confirmed is a guess. Confirm first.

## Procedure

1. Reproduce the cause at current HEAD, not from a remembered summary or a stale note.
2. Map every reader and writer of the state the fix would touch; enumerate the interleavings the fix would create.
3. Check the premise the fix depends on. If the fix assumes a value, field, or path exists, verify it does before designing against it.
4. Decide:
   - Confirmed: build the narrow fix.
   - Refuted: the diagnosis was wrong. Report the finding and ship no change. This is a result, not a failure.
   - Cannot prove safe: HOLD and write the tradeoff for a human.

## Why

The most expensive changes are correct fixes to the wrong problem, and plausible fixes built on an unchecked premise. A refutation that costs one investigation is far cheaper than a merge that has to be reverted — and the premise a fix quietly assumes is exactly the thing an investigation exists to catch.
