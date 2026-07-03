# fable-agent-orchestration

Apache-2.0 skill database for Fable-style agent orchestration.

The repo provides a public-clean set of reusable AI-agent workflow skills:
coordinate many coding agents, keep work isolated in git worktrees, validate
agent pull requests against real diffs and CI, and prevent fake-green tests.

This is intentionally generic. It does not include private project formulas,
identity material, private continuity-system details, third-party source, keys, or
closed operational notes.

## Contents

- `SKILL.md` - single-file Fable orchestration skill.
- `PUBLIC_GOAL_PLAYBOOK.md` - standalone public goal prompt/playbook for
  applying the orchestration pattern in other agent workspaces.
- `catalog.json` - machine-readable index of the clean skill database.
- `schemas/skill-record.schema.json` - schema for catalog entries.
- `skills/*/SKILL.md` - individual public-clean skills.

Current skill records:

- `fable-orchestrator`
- `autonomous-finish-loop`
- `think-work-try`
- `one-slice-worker-cycle`
- `two-critic-review-loop`
- `agent-pr-validator`
- `adversarial-reviewer`
- `task-relative-test-gate`
- `review-verifier`
- `orphaned-wip-adopter`
- `agent-dispatch-packet`
- `peer-review-packet`
- `fable-session-skill-miner`
- `external-workflow-adapter`
- `contributor-evidence-gate`
- `instruction-drift-control`
- `behavior-contract-harness`
- `phase-aware-engineering-ladder`
- `investigate-before-fix`
- `long-run-continuity`
- `easy-vs-right-check`
- `periodic-retrospect`
- `seal-both-types`

## Install

For the compact version, copy the root `SKILL.md` into your local skills folder:

```text
~/.claude/skills/fable/SKILL.md
```

For modular use, copy any folder from `skills/` into your agent's skills
directory.

## Core idea

The user or lead agent acts as a conductor. Agents do implementation slices.
The conductor owns scope, evidence, review, and merge decisions.

The reliable loop:

1. Split work into independent slices.
2. Launch one build agent per slice in an isolated git worktree.
3. Give every agent a packet with role, work scope, invariant, non-scope,
   proof gate, output contract, and rules.
4. Track every live worker in a parent registry so completion events are
   routed to the right task and verified against real artifacts.
5. Require each agent to open a pull request, not merge it.
6. Run two separate critics: one critic reviews the test gate, and one critic
   reviews the code change.
7. Verify the critics' claims against the current code and CI.
8. Merge one PR at a time on green evidence.
9. Relaunch the next slice while reviews and CI run.
10. Do not stop on a plan, promise, or background notification while a
    reversible next action remains.

The rule of thumb for delegation:

> Spawn an agent only when you will immediately go do something else.

If you would sit and wait, the agent adds overhead instead of throughput.

## Clean-public boundary

This repo includes only generic engineering workflow primitives:

- worktree isolation;
- fail-under-broken tests;
- sabotage or negative-control checks;
- two separate critic roles for tests and code review;
- PR claim-to-diff validation;
- CI and runtime evidence review;
- stale review detection;
- recovery of stalled work;
- autonomous finish-loop discipline for reversible in-scope work;
- label-stripping session mining for reusable skills;
- instruction-drift control for canonical agent guides, fix logs, and
  keep-in-sync contracts;
- behavior-contract harnesses for prompts, hooks, eval probes, telemetry, and
  real-work regression gates;
- phase-aware engineering ladders that scale verification depth without
  weakening the security and data-loss floor.

It excludes:

- private formulas or methodology names;
- private peer identities or memory systems;
- third-party source dumps or reverse-engineering artifacts;
- raw session transcripts or bulky logs;
- project secrets or credentials;
- private business, product, or architecture notes.

## Credits

This repository contains public-clean workflow skills and may adapt general
workflow ideas from permissively licensed projects. It does not vendor external
source code unless explicitly stated here.

### agent-standard-oss

- Source: <https://github.com/anmoln7/agent-standard-oss>
- License: MIT License
- Inspected commit: `263c7e8bbd2f337fcb1e0c352eaa2508192ce757`
- Used for: inspiration for the `instruction-drift-control` skill's public
  treatment of canonical agent instruction files, fix logs, keep-in-sync
  contracts, verifier decay, and autonomous-loop contracts.
- Vendored code: none.

The resulting skill is a rewritten synthesis for this repository's public
bounded-agent orchestration model, not a copy of the upstream scripts,
installer, hooks, or project instructions.

### opus-fable-playbook

- Source: <https://github.com/rennf93/opus-fable-playbook>
- License: MIT License
- Inspected commit: `160acb6a59f83d80e45ef8e1148bac28bd46c143`
- Used for: inspiration for the `behavior-contract-harness` skill's public
  treatment of behavior doctrine, stop gates, prompt nudges, honesty hooks,
  telemetry, critic agents, golden transcript evals, and measured variance.
- Vendored code: none.

The resulting skill is a rewritten synthesis for generic agent behavior
contracts. It does not copy upstream hooks, scripts, transcripts, prompts, or
project instructions.

### senior-engineering-partner

- Source: <https://github.com/bjgreenberg/senior-engineering-partner>
- License: Apache License 2.0
- Inspected commit: `8b4bb74600a3b49edcb9bb4a7f7522580a883040`
- Used for: inspiration for the `phase-aware-engineering-ladder` skill's public
  treatment of prototype/MVP/production rigor, fixed security floor, promotion
  triggers, and deferred gates.
- Vendored code: none.

The resulting skill is a rewritten synthesis for this repository's bounded
agent orchestration model. It does not copy upstream scripts, references, eval
scenarios, or project instructions.

## License

[Apache License 2.0](LICENSE).
