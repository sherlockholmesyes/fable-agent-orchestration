---
name: instruction-drift-control
description: Keep agent instruction files, project runbooks, and learned-fix notes single-sourced, current, and verifiable. Use when a project has AGENTS.md, CLAUDE.md, CODEX.md, README sections, skills, or fix logs that can drift apart.
---

# Instruction Drift Control

Use this when agent instructions are becoming a second, stale codebase.

The goal is not more documentation. The goal is one reliable instruction surface
plus small, searchable lessons that stay tied to real evidence.

## Apply When

- A repo has multiple instruction files such as `AGENTS.md`, `CLAUDE.md`,
  `CODEX.md`, `.cursor/rules`, skills, or README sections.
- An agent is likely to read one file while another file contains newer rules.
- A hard-won bug fix or workflow lesson keeps being rediscovered.
- A long-running agent engagement needs durable state across context resets.
- You are importing an external workflow or agent standard into a local repo.

## Do Not Use When

- The repo has one small, stable instruction file and no observed drift.
- The task is a one-off edit with no reusable lesson.
- The proposed entry cannot cite a real file, PR, issue, test, log, or failure.

## Core Rule

Instruction drift has two common failure modes:

- duplicate instructions in several files that slowly disagree;
- one large instruction file that becomes too bloated to read and maintain.

Use the middle path:

- one canonical instruction surface for current operating rules;
- thin pointers or adapter files for tool-specific entry points;
- separate fix-log entries for specific past bugs, gotchas, and workflow lessons;
- explicit sync contracts for files that must change together.

## Procedure

1. **Find the instruction surfaces.**
   List files and folders that can steer an agent:

   ```text
   AGENTS.md / CLAUDE.md / CODEX.md / README / skills/ / .cursor/
   project runbooks / CI workflow comments / docs/solutions/
   ```

2. **Choose the canonical surface.**
   Pick one file as the main operating guide. Other agent-specific files should
   be either:

   - a pointer to the canonical file; or
   - a thin adapter that contains only tool-specific mechanics.

   Do not copy the same rule into multiple files unless a sync contract pins the
   duplication.

3. **Split lessons from standing rules.**
   Standing rules belong in the canonical surface.

   Specific incidents belong in a fix log, one entry per file:

   ```md
   ---
   module: <area>
   tags: [<search terms>]
   problem_type: bug | gotcha | pattern | workflow
   date: YYYY-MM-DD
   ---

   ## Problem
   What went wrong.

   ## Cause
   Why it happened.

   ## Fix
   What to do next time.

   ## Re-check
   Command or artifact that verifies the lesson still applies.
   ```

4. **Add keep-in-sync contracts.**
   Add a small section naming file pairs that must move together:

   ```md
   ## Keep in sync

   - Add an env var -> update `.env.example` and the config docs.
   - Change a CLI flag -> update the README command example.
   - Change a public skill id -> update `catalog.json` and README.
   ```

   Prefer generated discovery over hand-kept inventories. If a list must be
   maintained by hand, give it a sync contract or a test.

5. **Define the autonomous-loop contract.**
   Before a long-running agent loop starts, write:

   - what it may touch;
   - how long or how much budget it may spend;
   - what counts as proof;
   - what it must record;
   - when a human or coordinator must be pulled in.

6. **Audit verifier decay.**
   A check that once proved the right thing can become stale. Periodically ask:

   - does the check still exercise the real path?
   - would the old broken behavior still fail?
   - has the surrounding code moved away from the check?
   - is the proof now only a screenshot, summary, or stale log?

7. **Keep session lessons public-clean.**
   When mining agent sessions, strip labels, private context, raw logs, secrets,
   and organization-specific details. Keep only the reusable procedure and the
   evidence anchor.

## Gates

Before accepting an instruction update:

- one canonical surface is clear;
- duplicated rules are removed or explicitly synced;
- every fix-log entry has `Problem`, `Cause`, `Fix`, and `Re-check`;
- every drift-prone pair has a keep-in-sync line;
- no secret, private path, raw transcript, or personal config is included;
- a zero-context agent can follow the instruction without guessing.

## Output

Return:

- canonical instruction surface chosen;
- files converted to pointers or thin adapters;
- fix-log entries added or updated;
- keep-in-sync contracts added;
- verification commands or artifacts checked;
- residual drift risks.

## Attribution

This skill is an adapted, public-clean synthesis inspired by
[`anmoln7/agent-standard-oss`](https://github.com/anmoln7/agent-standard-oss),
MIT licensed, inspected at commit
`263c7e8bbd2f337fcb1e0c352eaa2508192ce757`.

No code, scripts, installer, hooks, or raw project content from that repository
are vendored here. The useful public workflow primitives were rewritten for this
skill database's bounded-agent and evidence-gated workflow.
