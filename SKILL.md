---
name: fable
description: Orchestration playbook for driving a LARGE multi-change engagement (many PRs) as a coordinator running coding agents, not by hand-coding. Parallel build-agents in isolated git worktrees -> an adversarial review agent per PR -> a serialized merge-gate -> relaunch. Invoke when the work is bigger than one change (an audit-and-harden sweep, a plan with N slices, a backlog to burn down) and you should be conducting agents, not typing.
---

# fable -- the agent-orchestration playbook

Drive a large multi-slice engagement as the CONDUCTOR: you hold the conclusions and the gate; the agents hold the file-dumps. Distilled from driving ~two dozen agents to ~two dozen merged PRs in a single session.

**Outer loop / inner loop.** This playbook is the OUTER loop -- N slices: fan-out, gate, merge, tempo. Each spawned worker (and each delicate slice you keep yourself) runs the INNER loop on ONE slice -- **investigate -> build narrow -> prove by falsification** -- described near the bottom.

## The core loop (per slice)
1. **Launch a build-agent** in its OWN git worktree branched from the main branch -- `git worktree add <path-no-spaces> origin/main -b <prefix>/<name>`. Isolation = no file conflict between concurrent agents. Brief it with the inner worker-cycle; for a delicate / correctness-critical slice say so explicitly, so it investigates first and runs the sabotage negative-control rather than a happy-path patch. It opens a PR but DOES NOT MERGE.
2. **When it opens a PR, launch an independent review agent** focused on the ONE load-bearing risk this change touches (the invariant it could break), not a generic pass.
3. **Verify the review against ground truth** -- spot-check its load-bearing claims against the code at HEAD (a review can be stale: it may have read an intermediate commit). Reproduce a claimed fail-under-broken yourself when it is load-bearing; land real follow-ups IN the PR.
4. **Merge on PASS** (squash, delete branch), verify the main-branch CI is green.
5. **Relaunch** the next slice. Keep several build + review agents in flight at once; act on each completion -- don't sit and poll.

## When to spawn an agent vs do it yourself
The axis is **parallelism-gain**, not size. The one-line test: *"will I launch this and immediately go do something else?"*
- **Spawn a build-agent** when the slice is self-contained (its own worktree / file-region -> parallelizable without conflict) AND launching it lets you move straight to the next thing. That is the whole win: N runs while you advance N+1.
- **Do it yourself** when: it is a small mechanical delta you would finish faster than you would write the prompt (a config line, a doc row, a one-line guard); OR it is a load-bearing MERGE decision needing your full in-context judgment; OR it is the INVESTIGATION that de-risks a delegation -- do the recon, embed the decided design in a tight prompt, then delegate the implementation (a tight prompt also avoids the cold-start failures that fat, open-ended prompts trigger); OR an agent has died twice and hand-doing is now faster.
- **Always delegate the review** to an independent agent -- the point is eyes that are not yours.
- Don't spawn an agent you would only sit and wait for (no parallelism gain = pure overhead). Don't spawn past what you can gate (more agents than you can review + merge is a backlog, not throughput).

## The disciplines that keep it honest
- **One PR per invariant / concern**, with a fail-under-broken test that FAILS on the old code and exercises the REAL production path -- never a re-implementation of the logic inside the test.
- **Investigate-first** on any delicate / hot-path / security / recovery slice -- confirm the diagnosis or exposure BEFORE building. A REFUTATION (the diagnosis was wrong; open no PR) is a valid, valuable outcome. HOLD (don't auto-merge) a change to a core invariant you cannot prove safe under concurrency/crash.
- **Verify the reviewer** -- a review is a ruler; a "fix-first" can be stale and a "pass" can miss the one thing that hurts. Reproduce the load-bearing claim yourself.
- **Prefer structural guarantees over flags** -- a protection that depends on remembering to set a flag is not a guarantee; make it a property of the type/structure. In a typed contract, a "valid by construction" value must be unforgeable (seal both the accepted and the reject types so an external caller cannot fabricate an "accepted" value that skipped the check).
- **Parallel-worker-aware** -- another team/session may be working the same plan: review + merge their PRs under your gate instead of duplicating. If a session stalled (check the worktree file mtimes), adopt its coherent uncommitted work -- verify it by exercising it, rebase, finish, ship -- rather than rebuild.
- **Personal-verify small mechanical deltas** -- a mechanical mirror of an already-reviewed pattern gets your own eyes on the delta, not a redundant full review.
- **Handle cold-start failures** -- an agent may die immediately (zero tool calls; correlates with large prompts). Relaunch; if it dies twice, tighten the prompt (do the investigation yourself, embed the decided design) or do the slice by hand.
- **Capture findings immediately** in the task tracker (a residual, a security escalation, a refuted diagnosis) -- the fallback net.
- **Serialize merges** -- one PR at a time; rebase the next. A doc-only or same-region change still triggers a full CI cycle -- merge on green, not on the push.

## The inner worker-cycle (each slice): investigate -> build narrow -> prove
Three phases with HARD exits, not a mood. Each has a "you may not leave until" condition.
- **INVESTIGATE** (exit: the real hazards named, or the diagnosis refuted). Read the REAL code at HEAD, not your memory of it. For a state-machine change, map EVERY reader/writer of the touched state (grep the field, list all sites), then enumerate the interleavings the change creates -- what can run in the new window, what takes the lock, what rewrites the file, what counts the queue. The packet names ONE hazard; the investigation must find the ones it does NOT name. A refutation is a valid exit (write HELD/BLOCKED with the tradeoff).
- **BUILD NARROW** (exit: one rule, narrow diff). Consider the tempting-but-wrong approaches -- the naive fix, the big rewrite, the adjacent refactor, do-nothing -- and cut them honestly; the right answer is usually the narrow one none of them names, from which every hazard-fix follows structurally instead of as separate patches. If your fixes don't collapse into one rule, you may be patching symptoms. Keep every existing call-site bit-identical; prefer proofs the compiler holds (a phase that must not touch state takes no mutable reference -- the signature is the proof). Keep gate-state in data, never in a remembered flag.
- **PROVE** (exit: gates + review green). One falsification test per hazard, exercising the SAME production functions the real entrypoint calls (anti-tautology), including the crash-point ruler (recovery reproduces at least everything observable). Then the **sabotage negative-control** (the step most skipped): commit the real change, then build the broken variants (revert each protection) and demand EXACTLY the matching tests fail while the structural ones stay green -- N-broken/M-green is a fingerprint; all-green on a sabotaged build means the tests are theater. Then the full gate battery + an independent review briefed to REFUTE (attack angles listed, line-numbers demanded), then verify its verdict, then merge.

## The tempo: never stop, keep the flywheel spinning
The style that makes it many PRs, not a few: **continuous bounded flow.** Every turn ends with work IN FLIGHT -- never idle, never a dead stop. The flywheel: launch -> while it runs, advance something ELSE (review a returned PR, merge a green one, do a small fix by hand, recon the next slice) -> act on the next completion immediately -> re-arm. Bounded: as fast as you can still GATE (review + merge) each result; past that is backlog, not speed.

Two guards against the flywheel stopping:
- **Don't end a turn on a plan, a question, or "waiting"** when an action is available -- do the action (retry the error, gather the info yourself, launch the next). A last paragraph that is a promise ("I'll...") is a miss; do it now.
- Don't hold for the human when a reversible next step follows from the ask -- that is stopping-too-early in a polite mask.

## Scaling past your own agents: dispatch to worker sessions
When the backlog exceeds what you should run yourself: (1) analyze the critical / hardest / most-expensive nodes of the system; (2) write a shared queue file (use an ASCII path -- some message channels choke on non-ASCII) with per-task packets (files / invariant / acceptance / fail-under-broken / non-scope) and a protocol (do -> merge -> write a `<task-id>.done` report file); (3) hand the tasks ONE AT A TIME to a worker session; (4) watch the report folder so you are notified when each lands, then feed the next.

The one thing this playbook exists to prevent: hand-coding slice N while slices N+1..M sit idle. Conduct -- fan out, gate hard, merge clean, repeat.
