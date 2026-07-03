---
name: seal-both-types
description: In a typed contract where holding a value is meant to prove it passed a check, "valid by construction" is false unless the value is unforgeable. Seal both the accepted (positive) type and the reject type against external construction, so no caller can fabricate an "accepted" value that skipped the check.
---

# Seal Both Types

A "possession proves validity" claim is only as strong as the caller's inability to forge the value.

## The trap

It is common to seal the reject/error type but leave the accepted type an open struct with public fields. Then any caller can construct an "accepted" value directly, skipping the check the type was supposed to prove — and the "valid by construction" guarantee is a lie.

## The rule

Seal both sides of the contract:

- the reject type, and
- the accepted / positive type.

Make the accepted value constructable only through the checking function: no public field literal, no default constructor, no deserialization path that bypasses the check. In Rust this is a non-exhaustive type with private fields and no public constructor; other languages have equivalents — a private constructor behind a factory, a sealed class, an opaque type.

## Test

Try to construct an accepted value in a downstream module without calling the checker. If it compiles, the seal is incomplete.
