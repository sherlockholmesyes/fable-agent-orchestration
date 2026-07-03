---
name: behavior-contract-harness
description: Build or review an executable agent behavior contract using prompts, hooks, telemetry, eval probes, and real-work gates. Use when adopting Fable-like doctrine, stop gates, honesty nudges, output styles, golden transcript evals, or any plugin that claims to make an agent behave better.
---

# Behavior Contract Harness

Use this when an agent workflow should be enforced and measured, not merely
described.

The goal is not to make a model sound like another model. The goal is to turn
desired behavior into a contract with enforcement points, eval probes, real-work
evidence, and regression gates.

## Failure Modes

Avoid both:

- **Style cosplay**: the agent sounds closer to the target behavior but still
  stops too early, buries failures, or claims unverified success.
- **Hook theater**: the workflow blocks obvious bad endings but has no evidence
  that it improves real task completion.

The useful shape is:

```text
behavior rule -> enforcement surface -> eval probe -> negative control
-> real-work telemetry -> regression decision
```

## Procedure

1. **Name the behavior contract.**
   Define the behavior in operational terms:
   - finish reversible in-scope work;
   - lead with the outcome;
   - report failures verbatim;
   - distinguish questions from implementation requests;
   - use parallelism only when it creates throughput;
   - verify before claiming done.

2. **Pick enforcement surfaces.**
   Choose the smallest surfaces that can catch drift:
   - prompt or instruction text for posture;
   - stop hook for promised-but-undone final messages;
   - prompt hook for question-shaped requests;
   - post-tool hook for failed command honesty;
   - pre-compact hook for continuity preservation;
   - critic agent for nontrivial completion claims.

3. **Classify authority.**
   Every enforcement surface must declare:
   - read-only nudge;
   - blocking gate;
   - LLM judge;
   - telemetry-only observation.

   Prefer fail-open for tooling errors. Prefer fail-closed only when a false
   pass would ship an unsafe or misleading claim.

4. **Build eval probes.**
   Each probe should have:
   - task prompt;
   - expected behavior checklist;
   - candidate output;
   - baseline output;
   - judge rubric;
   - negative control where possible.

   Do not use golden transcript similarity alone. Add real outcome checks:
   task completed, test fixed, diff correct, failure reported, no false done.

5. **Measure noise before tuning.**
   Run repeated baseline and candidate trials. Treat small deltas as noise until
   they exceed measured run-to-run spread and at least one-probe-flip magnitude.

6. **Run real-work telemetry.**
   Synthetic probes are not enough. Track local-only counts for:
   - stop-too-early caught;
   - failed-command honesty nudges;
   - tool discipline catches;
   - critic refutations;
   - false positives;
   - time and token overhead.

7. **Decide by regression gate.**
   Accept a contract change only when it improves a named behavior without
   increasing false positives or blocking correct work beyond the declared
   budget.

## Required Negative Controls

Include probes where the correct behavior is to stop or ask:

- destructive action without clear user request;
- credential, payment, publish, or irreversible external gate;
- question-shaped prompt that asks for assessment, not edits;
- implementation request with no reversible next step available;
- failed tests where the correct final answer must report failure.

## Review Checklist

Before adopting a behavior plugin or contract:

- license is compatible;
- telemetry is local-only or explicitly disabled;
- hooks are readable and bounded;
- shell scripts have LF line-ending protection;
- Windows, WSL, macOS, and Linux limits are documented;
- evals isolate unrelated plugins and settings;
- goldens or reference outputs are not treated as absolute truth;
- real-work smoke tests exist;
- false-positive budget is stated;
- no private project data, raw session logs, secrets, or closed methodology is
  included.

## Output

Return:

- behavior contract being enforced;
- enforcement surfaces and authority class;
- eval probes and negative controls;
- real-work telemetry or smoke test evidence;
- accepted changes, rejected changes, and residual risks.

## Attribution

This skill is an adapted, public-clean synthesis inspired by
[`rennf93/opus-fable-playbook`](https://github.com/rennf93/opus-fable-playbook),
MIT licensed, inspected at commit
`160acb6a59f83d80e45ef8e1148bac28bd46c143`.

No code, hooks, scripts, golden transcripts, or raw project content from that
repository are vendored here. The useful public workflow primitives were
rewritten as a generic behavior-contract harness for this skill database.
