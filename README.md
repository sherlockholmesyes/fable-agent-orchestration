# fable — an agent-orchestration skill for Claude Code

A [Claude Code](https://claude.com/claude-code) skill: an orchestration playbook for driving a **large multi-change engagement** (many pull requests) as a *coordinator running coding agents*, rather than hand-coding every change yourself.

It was distilled from a single session that drove roughly two dozen agents to roughly two dozen merged PRs, and it captures the loop — and the disciplines — that kept that honest.

## The idea

You are the **conductor**: you hold the conclusions and the merge-gate; the agents hold the file-dumps.

- **The core loop** — launch a build-agent per slice in its own git worktree → an independent review agent per PR → verify the review against ground truth → merge on pass → relaunch.
- **When to spawn an agent vs do it yourself** — the axis is *parallelism-gain*, not size: "will I launch this and immediately go do something else?"
- **The disciplines that keep it honest** — one PR per concern, a fail-under-broken test, investigate-first, verify-the-reviewer, structural-guarantees-over-flags, sabotage negative-controls, and more.
- **The inner worker-cycle** — investigate → build narrow → prove by falsification.
- **The tempo** — continuous bounded flow: every turn ends with work in flight; never idle, never a dead stop.
- **Scaling** — how to dispatch tasks to worker sessions when the backlog exceeds your own agents.

## Install

Copy `SKILL.md` into a `fable/` folder under your Claude Code skills directory:

```
~/.claude/skills/fable/SKILL.md
```

Claude Code discovers it automatically. It fires when a task is bigger than one change and you should be conducting agents rather than typing.

## License

[Apache License 2.0](LICENSE).
