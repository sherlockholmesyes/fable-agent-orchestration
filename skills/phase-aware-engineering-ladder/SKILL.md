---
name: phase-aware-engineering-ladder
description: Calibrate an implementation, review, or agent packet to the project's phase without dropping the security and data-loss floor. Use when choosing between prototype, MVP, and production rigor, writing deferred gates, reviewing lean builds, or preventing both overengineering and unsafe shortcuts.
---

# Phase-Aware Engineering Ladder

Use this when the right amount of engineering rigor depends on project phase.

The goal is not to apply the full production checklist to every throwaway
experiment, and not to let "prototype" become an excuse for unsafe defaults.
Verification depth scales. The floor does not.

## Failure Modes

Avoid both:

- **Production theater**: a small spike gets blocked by expensive ceremony that
  does not improve the current risk.
- **Prototype laundering**: real users, money, public exposure, or persistent
  data are treated as "just a prototype" so security, backup, and validation
  controls are skipped.

The useful shape is:

```text
phase -> fixed floor -> scaled gates -> explicit deferred gates
-> promotion triggers -> verification result
```

## The Fixed Floor

These are required at every phase:

- no hardcoded secrets;
- input validation at trust boundaries;
- no shell, SQL, template, CSV, log, or prompt injection;
- isolated dev/test environment;
- authentication for exposed surfaces;
- least privilege for credentials and deploy tokens;
- dependency vetting before adoption;
- a data-loss story for anything that stores or produces valuable data;
- restore or recovery check appropriate to the value of the data;
- failure is visible, not silent.

If a task cannot keep the floor, do not hide it behind "MVP" or "quick hack".
State the blocker and choose a safer shape.

## Phase Ladder

### Tier 0: Prototype or Spike

Use for throwaway learning, demos, or risk probes that do not hold real user,
tenant, production, or business-critical data.

Required:

- fixed floor;
- README or note explaining purpose and how to run;
- minimal test or manual proof of the explored claim;
- cleanup path or explicit "throwaway" marker.

May defer:

- full CI matrix;
- coverage or mutation gates;
- formal threat model;
- load test;
- production runbook.

### Tier 1: MVP or Early Product

Use once there are real users, small-scale production use, public access,
money-adjacent workflows, or durable data.

Required:

- fixed floor;
- critical-path tests;
- basic CI;
- secret scanning or equivalent check;
- pinned and locked dependencies where practical;
- structured failure visibility;
- backup or recovery procedure for valuable data;
- explicit deferred production gates with promotion triggers.

May defer only with a written trigger:

- full load ladder;
- formal incident process;
- complete compliance mapping;
- multi-region or high-availability design;
- mutation/property/fuzz test expansion.

### Tier 2: Production, Commercial, Multi-Tenant, or Regulated

Use for paid products, multi-tenant systems, public production services,
regulated or sensitive data, or any system where failure harms users or the
business.

Required:

- Tier 1 plus production gates;
- threat model for exposed or sensitive surfaces;
- robust test suite for critical behavior;
- authorization and tenant-isolation tests where relevant;
- dependency audit and supply-chain checks;
- observability and alerting;
- backup restore drill or equivalent recovery proof;
- rollback plan;
- release and merge gates that block unsafe changes.

## Promotion Triggers

Move up the ladder immediately when any trigger appears:

- real users;
- money changing hands;
- public internet exposure;
- persistent valuable data;
- multi-tenant isolation;
- regulated, personal, or sensitive data;
- second contributor or agent fleet;
- scheduled unattended operation;
- customer-facing promise.

Promotion is a risk change, not polish.

## Procedure

1. **Name the phase.**
   If ambiguous, choose the lowest phase that honestly matches exposure and
   data value, then list what would promote it.

2. **Pin the floor.**
   State how the fixed floor is satisfied or where it is blocked.

3. **Choose scaled gates.**
   Pick the proof appropriate to the phase:
   - Tier 0: one direct proof or focused test;
   - Tier 1: critical-path tests plus basic CI;
   - Tier 2: task-relative tests, security checks, recovery or rollback proof,
     and review gates.

4. **Write deferred gates as obligations.**
   Deferred work must include:
   - what is deferred;
   - why it is acceptable now;
   - exact trigger that makes it mandatory;
   - owner or tracking location.

5. **Review shortcuts.**
   Reject shortcuts that weaken the fixed floor. Accept shortcuts that only
   reduce phase-scaled verification depth.

6. **Report the decision.**
   Include:
   - phase chosen;
   - floor status;
   - gates run;
   - deferred gates and triggers;
   - residual risk.

## Agent Packet Add-On

When delegating work, include this block:

```text
Phase: Tier 0 / Tier 1 / Tier 2
Fixed floor:
  satisfied / blocked / non-applicable with reason
Scaled gates:
  tests, CI, review, security, recovery, runtime proof
Deferred gates:
  item -> trigger -> owner/path
Promotion triggers:
  conditions that require rerating the slice
```

## Attribution

This skill is an adapted, public-clean synthesis inspired by
[`bjgreenberg/senior-engineering-partner`](https://github.com/bjgreenberg/senior-engineering-partner),
Apache-2.0 licensed, inspected at commit
`8b4bb74600a3b49edcb9bb4a7f7522580a883040`.

No code, scripts, references, eval scenarios, or raw project content from that
repository are vendored here. The useful public workflow primitives were
rewritten as a compact phase-aware engineering ladder for this skill database.
