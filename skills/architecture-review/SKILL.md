---
name: architecture-review
description: Sniffs bad smells in module boundaries, dependency directions, and layering across a codebase (source structure, import graphs, optional architecture diagrams or layering ADRs) without rewriting them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a new module crosses existing boundaries, when a refactor reshapes dependency directions, when an architecture artifact is up for review, or when a codebase has no enforced architectural rules and needs a coherence pass.
phase: Review
---

# architecture-review

## Overview

A graybox audit skill for module-level architectural relationships. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits whether dependency directions and module boundaries are architecturally sound: layering, abstraction levels, module responsibility cohesion. The substance of whether the chosen architecture is the right strategic choice for the product (positioning) and whether internal complexity within a module is acceptable (size, depth, duplication) belong to other audit domains.

This audit complements rather than replaces configured architectural enforcement tooling (ArchUnit, dependency-cruiser, madge, tach, eslint-plugin-boundaries). Many concerns here are semantic — abstraction quality, responsibility cohesion, ADR-vs-code drift — that tools cannot enforce until rules are explicitly authored, and several that tools cannot enforce at all. The audit is most valuable when no such tooling is configured, or as a complement to tooling that catches only declared violations.

## When to Use

- A new module or layer is introduced and its placement crosses existing boundaries
- A refactor reshapes dependency directions or merges/splits modules
- An architecture artifact (ADR on layering, hexagonal/clean diagram, dependency policy) is up for review
- A codebase has no enforced architectural rules and a coherence pass is needed

**When NOT to use:** within-module structural complexity such as long functions, deep nesting, or duplication (that's `complexity-review`'s domain); the external API contract a module exposes (that's `interface-contract-review`'s domain); whether documented architectural claims (ADR / diagram / README) match the code (that's `doc-alignment-review`'s domain; when no explicit doc exists, architecture-review still covers cases where folder structure claims a layering that actual imports do not respect); evaluating whether the architecture is the right strategic choice for the product (positioning conversation, not an audit).

## Smell Profile

Named categories used as report vocabulary:

- **Dependency direction violation** — high-level policy / use-case / domain module directly depends on low-level IO / DB / framework concretion without an inverted abstraction; the dependency points the wrong way for the layering claim
- **Layer skip** — code crosses intermediate layers (UI directly accesses DB, controller directly uses ORM, domain module directly imports HTTP client) without going through the intended intermediate
- **Cyclic module dependency** — modules import one another forming a cycle, making them not separately reasonable, testable, or deployable
- **Abstraction leak** — high-level interface signature exposes low-level implementation types (domain interface returns ORM Row, public method takes a file-path string instead of a stream, repository returns a SQL cursor)
- **Boundary erosion** — utility / infra module references domain concepts; domain module mixes in IO / transport / serialization details that belong in adapter modules; or external code reaches into another module's marked-internal members (private / protected / `_underscore` / unexported equivalent, or reflection bypass) instead of going through the public API
- **God module** — a single module accumulates heterogeneous responsibilities (auth + caching + logging + parsing co-located) so its role cannot be stated without "and"
- **Anti-corruption layer absence** — integration with an external system imports its model directly into internal domain code without a translation / adapter layer
- **Implicit boundary** — folder structure presents an architecture (`domain/`, `infra/`, `api/`) but actual imports cross those boundaries freely; the boundary is decorative
- **Stable dependencies violation** — a frequently-changing module is depended on by a stable module, forcing churn into modules that should be stable

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual code under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a module path, import statement, layer name, or directory). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

Source code structure (module / package / folder layout, import statements, public interface declarations) as the primary artifact, with optional cross-reference to architectural ADRs, layering diagrams, or README descriptions of the intended structure when those are in the in-scope set. Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the in-scope source and architectural documents only — no live runtime introspection, no external dependency analysis, no third-party docs.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Architecture Review — <subject>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/architecture-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as module path, import statement, or layer name.>

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

- Within-module structural complexity (function size, nesting, duplication)
- External API contract evaluation
- Drift between documented architectural claims and code (ADR / diagram / README vs import graph)
- Strategic judgment on whether the chosen architecture is right for the product
- Performance characteristics of the dependency graph
- Recommendations to add, remove, or restructure modules beyond surfacing the smell
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
