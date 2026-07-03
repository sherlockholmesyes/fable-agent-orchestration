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
- `catalog.json` - machine-readable index of the clean skill database.
- `schemas/skill-record.schema.json` - schema for catalog entries.
- `skills/*/SKILL.md` - individual public-clean skills.

Current skill records:

- `fable-orchestrator`
- `one-slice-worker-cycle`
- `agent-pr-validator`
- `adversarial-reviewer`
- `task-relative-test-gate`
- `review-verifier`
- `orphaned-wip-adopter`
- `peer-review-packet`
- `external-workflow-adapter`

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
3. Require each agent to open a pull request, not merge it.
4. Review each PR with an independent critic focused on the invariant at risk.
5. Verify the critic's claims against the current code and CI.
6. Merge one PR at a time on green evidence.
7. Relaunch the next slice while reviews and CI run.

The rule of thumb for delegation:

> Spawn an agent only when you will immediately go do something else.

If you would sit and wait, the agent adds overhead instead of throughput.

## Clean-public boundary

This repo includes only generic engineering workflow primitives:

- worktree isolation;
- fail-under-broken tests;
- sabotage or negative-control checks;
- PR claim-to-diff validation;
- CI and runtime evidence review;
- stale review detection;
- recovery of stalled work.

It excludes:

- private formulas or methodology names;
- private peer identities or memory systems;
- third-party source dumps or reverse-engineering artifacts;
- project secrets or credentials;
- private business, product, or architecture notes.

## License

[Apache License 2.0](LICENSE).
