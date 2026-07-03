---
name: long-run-continuity
description: Keep a long multi-PR engagement recoverable across a context or session boundary. Checkpoint the live queue, in-flight pull requests, and open findings to durable storage so nothing is silently dropped when working context is lost. The durable save is the guarantee, not any in-context note.
---

# Long Run Continuity

A large engagement will outlive the working context it started in. Assume that context can be lost at any point, and make the run survive it.

## Checkpoint

Persist to durable storage — a task tracker, a committed notes file, an issue — not just working memory:

- the queue: what is done, in flight, and not yet started;
- every open pull request and its evidence state;
- open findings, residuals, and security escalations, recorded the moment they surface;
- pending decisions and their blockers.

## Rules

- Capture a finding when you find it, not at the end. The end may not arrive in the same context.
- The durable write is the guarantee. An in-context note that a boundary erases is not a checkpoint.
- Write state a fresh reader can act on with no prior context: exact paths, PR numbers, the precise next step.

## When

Continuously through a long run, and always before a step that could exhaust or reset the working context.
