---
name: fable-session-skill-miner
description: Mine Fable-style agent session logs, reports, or repeated orchestration mechanics into public-clean reusable skills. Use when a session appears to contain a durable procedure, but labels, private context, and raw logs must not leak into the skill.
---

# Fable Session Skill Miner

Mine procedures, not labels.

## Retention Gate

A candidate survives only when all checks pass:

1. Label stripping: remove names, slogans, tool brands, project labels, and
   private terminology. The remaining procedure must still be useful.
2. Existing-skill comparison: name the closest existing skill and choose import,
   adapt, or reject.
3. Evidence anchor: cite a public-safe report path, PR, issue, or line range.
   Do not copy raw transcripts.
4. Procedure shape: apply-when, steps, non-scope, and validation gate are
   explicit.
5. OPSEC: no secrets, private paths, private identities, private formulas,
   proprietary docs, third-party source dumps, or bulky logs.

If the candidate lacks a validation gate, it is a note, not a skill.

## Procedure

1. Identify the repeated behavior or workflow mechanic.
2. Strip labels and private context.
3. Compare against the existing catalog.
4. Decide:
   - update an existing skill;
   - create a new skill;
   - record as a non-skill note;
   - reject as label-only.
5. Write the smallest public-clean skill body that preserves the procedure.
6. Update the catalog and README when a new skill is added.
7. Validate the manifest and scan for OPSEC leaks.

## Output

Return:

- candidate inspected;
- closest existing skill;
- decision;
- files changed;
- validation run;
- rejected labels or leak risks.
