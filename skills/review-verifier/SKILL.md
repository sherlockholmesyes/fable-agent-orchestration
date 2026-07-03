---
name: review-verifier
description: Verify an adversarial review before acting on it. Use when a reviewer says PASS, FIX_FIRST, REQUEST_CHANGES, or identifies a critical bug; spot-check load-bearing claims against current HEAD before merging or fixing.
---

# Review Verifier

A review is a signal, not ground truth. Verify the reviewer.

## Procedure

1. List the review's load-bearing claims.
2. Open the cited files at the current head commit.
3. Confirm whether each finding is real, stale, wrong, or non-blocking.
4. For a PASS, spot-check the one or two claims that would hurt most if wrong.
5. For a FIX_FIRST, reproduce or inspect the failure before changing code.
6. If real, land follow-ups in the same PR when scoped.
7. If stale or wrong, record why and proceed.

## Output

- `REAL_BLOCKER`: fix before merge.
- `REAL_NON_BLOCKING`: file or land small follow-up.
- `STALE`: reviewer read old code.
- `WRONG`: claim does not match current code.
- `CONFIRMED_PASS`: sampled claims checked out.
