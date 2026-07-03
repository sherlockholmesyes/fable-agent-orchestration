---
name: peer-review-packet
description: Build a small clean packet for external model or peer review of a structural engineering decision. Use when outside correlation is useful, but dumping the workspace or private context would be noisy or unsafe.
---

# Peer Review Packet

External review is advisory. Local evidence remains the gate.

## Packet Shape

Include:

- one explicit question;
- current verified state;
- exact files, functions, PRs, tests, or logs relevant to the question;
- constraints and non-scope;
- requested output format;
- repository URL if public state matters.

Exclude:

- secrets, tokens, credentials;
- private identity or memory details;
- third-party source dumps or reverse-engineering artifacts;
- unrelated historical context;
- private methodology names.

## Intake

When the answer returns:

1. Separate facts, inferences, and recommendations.
2. Verify factual claims locally.
3. Convert useful recommendations into tests, review tasks, or issues.
4. Reject generic advice that violates the current constraints.
5. Save only durable findings.
