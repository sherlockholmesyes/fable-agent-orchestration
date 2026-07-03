---
name: contributor-rigor-pack
description: Keep public repository contributions narrow, current, evidence-backed, and easy to review. Use before editing a public repo, preparing an AI-assisted patch, or writing a pull request that needs a clean task contract, live-state check, scope fence, adversarial check, and concise report.
---

# Contributor Rigor Pack

Use this when a public repository contribution must be small enough to review
and concrete enough to prove. The goal is not to make a patch look disciplined.
The goal is to prevent broad, stale, or unverifiable work from entering the
queue.

## When To Use

Use for:

- small public issues;
- AI-assisted pull requests;
- documentation changes that mention current behavior;
- test or CI changes;
- contributor task packs;
- external patches that need a clear review contract.

Do not use as a substitute for the repository's actual tests, CI, security
review, or release gates.

## The Loop

1. Check current truth.
   - Read the current files, branch, open issue, and relevant tests.
   - Do not rely on stale README text when code or CI can answer the question.

2. Write the patch contract before editing.

   ```text
   Task:
   Files expected:
   Non-scope:
   Acceptance command:
   Risk if wrong:
   ```

3. Fence the scope.
   - Change only the files needed for the task.
   - Put nearby issues in `Noticed, not changed`.
   - Do not silently refactor adjacent areas.

4. Build one narrow change.
   - One behavior, contract, or documentation correction per pull request.
   - If the patch needs two unrelated proofs, split it.

5. Prove the task, not the vibe.
   - Run the command that would catch the actual failure.
   - A generic green check is not enough if it does not exercise the changed
     behavior.

6. Run an adversarial pass.
   - Name one realistic way the patch could still be wrong.
   - Check that risk against the diff, test, generated artifact, or live output.

7. Keep public memory clean.
   - Do not commit secrets, private notes, raw prompts, raw model outputs,
     bulky logs, or local-only paths unless the repo explicitly asks for them.
   - Persist durable facts, not chat residue.

8. Report compactly.
   - Keep the final pull request summary short.
   - Lead with what changed and how it was proved.

## Pull Request Shape

```text
Task:
Files changed:
Non-scope:
Proof command:
Proof result:
Adversarial check:
Noticed, not changed:
```

## Review Gates

Ask these before submitting:

- Would this patch still look correct if the README were stale?
- Would the proof fail if the changed behavior were broken?
- Did the patch change only what the task required?
- Did it avoid raw secrets, raw prompts, raw outputs, and local-only private
  material?
- Can a reviewer reproduce the claim from the repo and the listed command?

## Failure Modes

Reject or split the work when:

- the patch combines feature work, refactoring, generated artifacts, and docs;
- the proof is only "tests pass" but the task is more specific;
- the PR summary hides residual risk;
- the patch makes a capacity, security, or reliability claim without a measured
  gate;
- the contribution imports external workflow text as authority instead of
  rewriting it for this repo's public rules.

## Output

Return:

```text
Patch contract:
Changed files:
Proof:
Adversarial check:
Residuals:
Next smallest task:
```
