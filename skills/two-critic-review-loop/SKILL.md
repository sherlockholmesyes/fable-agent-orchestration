---
name: two-critic-review-loop
description: Validate an agent pull request with two separate critics: one test critic for the proof gate and one adversarial change critic for the implementation. Use before merging non-trivial agent-generated PRs or release claims.
---

# Two Critic Review Loop

Strengthen the verifier, not the generator. One reviewer is not enough for
non-trivial work because proof quality and implementation quality fail in
different ways.

## Critic 1: Test Gate Critic

This critic reviews the ruler.

Ask:

- What exact task trajectory is being tested?
- Would this gate fail under the old broken behavior?
- Does it exercise the production path rather than a reimplementation?
- Was the gate weakened to turn green?
- Is there a broken arm and a fixed arm, or only a partial proof?

Verdicts:

- `TEST_PASS`
- `TEST_FAIL`
- `TEST_SPLIT`

## Critic 2: Adversarial Change Critic

This critic reviews the change.

Ask:

- Does the code actually close the claimed invariant?
- Does it introduce ownership, durability, security, compatibility, or product
  regressions?
- Does the property follow structurally, or only through a flag, timing sleep,
  cleanup ritual, or wrapper process?
- Are residual risks explicit and scoped?

Verdicts:

- `CHANGE_PASS`
- `CHANGE_FIX_FIRST`
- `CHANGE_REJECT`
- `CHANGE_SPLIT`

## Conductor Gate

Do not merge until:

1. the test critic passes or an explicit partial-proof residual is accepted;
2. the change critic passes or all blocking follow-ups are landed;
3. the conductor spot-checks both critics against current code and CI;
4. the PR body records scope, non-scope, proof, and residuals.

## Output

Return:

- test critic verdict;
- change critic verdict;
- conductor spot-check result;
- merge decision;
- smallest next follow-up.
