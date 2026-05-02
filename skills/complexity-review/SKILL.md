---
name: complexity-review
description: Sniffs bad smells in source code for structural complexity (long function, deep nesting, duplication, premature abstraction, dead code) without executing it. Reports the few concentrations that matter; holds full evidence in working memory. Use when a code change is up for review before merge, when an inherited module is being onboarded, or when reviewing a contributor's source contribution.
phase: Review
---

# complexity-review

## Overview

A graybox audit skill for source code structural complexity. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural shape of code: function and module size, nesting depth, duplication, abstraction earning its keep, naming clarity. The substance of whether the algorithm is correct (correctness review), whether it is fast enough (performance review), and whether it is secure (security review) belong to other audit domains.

## When to Use

- A code change (PR / patch / module) is up for review before merge
- An inherited module is being onboarded and a structural baseline is needed
- A contributor's source contribution is up for review

**When NOT to use:** evaluating algorithm correctness or business logic (correctness review, not a structural audit); checking runtime performance (that's `performance-review`'s domain); checking security posture (that's `security-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Long function** — a function exceeds local norms without internal decomposition or rationale for length
- **Deep nesting** — control flow nesting depth obscures the dominant path; guard clauses or extraction would flatten
- **Duplicated logic** — same logical step appears in multiple places without a shared abstraction
- **Large class / large module** — one unit aggregates many responsibilities; cohesion is low
- **Dead code** — function, branch, parameter, or import is unreachable or no longer referenced
- **Premature abstraction** — abstraction (interface, base class, factory, hook) wraps a single concrete use with no second consumer
- **Speculative generality** — parameters, options, or extension points are exposed but never used by any caller
- **Long parameter list** — function takes many positional or required parameters that are not naturally cohesive
- **Cyclic dependency** — modules / classes / packages reference each other, directly or transitively
- **Naming opacity** — identifier (function, variable, class, file) does not reflect what it does or contains; abbreviation without dictionary

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual code under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a file path, function name, line range, or symbol). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

Source code (one or more source files in any language, a diff/patch, a single module). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the source artifact(s) only — no live execution, no test results, no external links — with cross-reference limited to in-scope files.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Complexity Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/complexity-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as file path, function, or line range.>

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

- Algorithm correctness or business logic judgments
- Recommendations to add, remove, or restructure code beyond surfacing the smell
- Runtime performance evaluation (timing, allocation, complexity-class measurement)
- Security or threat-model evaluation
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
