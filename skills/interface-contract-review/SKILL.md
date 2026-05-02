---
name: interface-contract-review
description: Sniffs bad smells in interface contracts (OpenAPI, JSON Schema, protobuf, GraphQL, TypeScript type modules) without opening the box. Reports the few concentrations that matter; holds full evidence in working memory. Use when a contract is finalized before implementation, when a long-running spec has accumulated changes, or when reviewing a contributor's contract artifact.
phase: Build
---

# interface-contract-review

## Overview

A graybox audit skill for interface contracts. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a contract: schema completeness, boundary specification, naming, error responses, evolvability. The substance of whether the chosen contract is the right contract (API design judgment) and whether the implementation actually conforms (drift detection) belong to other audit domains.

## When to Use

- A new contract is finalized before implementation kickoff
- A long-running spec has accumulated changes and needs a coherence pass
- A contributor's contract artifact is up for review before merge

**When NOT to use:** reviewing API design philosophy or tech stack choices (positioning conversation, not an audit); checking whether the implementation matches the contract (that's `doc-alignment-review`'s domain); auditing prose documentation about the contract (that's `design-doc-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Schema completeness gaps** — an endpoint or method has no schema, or the schema covers only the happy path with response variants undefined
- **Boundary condition silence** — fields lack specification for null, empty, extreme, or boundary values
- **Naming inconsistency** — the same concept is named differently across endpoints (e.g., `userId` / `user_id` / `uid`) or different concepts share a name
- **Error response gaps** — endpoints define only success responses; 4xx, 5xx, auth failure, or validation error shapes are absent
- **Field evolution unsafe** — no versioning strategy, no deprecation markers, optional/required transitions undefined
- **Pagination convention scattered** — multiple pagination styles (offset/cursor/page) appear in one contract without rationale
- **Auth requirement unclear** — endpoints lack auth scope/role specification, or scope requirements vary without documentation
- **Idempotency unspecified** — non-GET endpoints lack idempotency contracts (key, retry semantics)
- **Type-precision mismatch** — primitives are over-specified (raw string for structured ID) or under-specified (open object for typed payload)

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual contract, not a pre-authored combination. Each observation carries a category tag and an evidence anchor. Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A contract artifact (OpenAPI YAML/JSON, JSON Schema, protobuf `.proto`, GraphQL SDL, TypeScript type module, or equivalent). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the contract document(s) only — no source code implementation, runtime traffic, or external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Interface Contract Review — <contract name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/interface-contract-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as endpoint path, schema name, or line range.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All named categories were scanned. Observations beyond the top concentrations are held in working memory; any category, location, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Judgments on API design choices (REST vs GraphQL, resource naming philosophy, version strategy preferences)
- Recommendations to add, remove, or restructure endpoints
- Cross-checks against source-code implementation
- Performance, security, or reliability evaluation of the contract's runtime behavior
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
