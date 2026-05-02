---
name: doc-alignment-review
description: Sniffs bad smells in documentation alignment with the artifacts it documents (README, API reference, ADR, architecture overview, runbook against in-scope spec or code). Reports the few concentrations that matter; holds full evidence in working memory. Use when a doc set is up for review, when a long-running doc has drifted, or when reviewing a contributor's documentation contribution.
phase: Ship
---

# doc-alignment-review

## Overview

A graybox audit skill for documentation alignment. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of documentation as it relates to the artifacts it documents (in scope): drift between doc and spec/code, ADR completeness, audience clarity, link health, maintenance signals. The substance of whether the doc is well-written prose (writing-quality judgment) and whether it covers the right things at the philosophical level (information architecture judgment) belong to other audit domains.

## When to Use

- A documentation set is up for review before merge
- A long-running doc has drifted and a coherence pass against the current spec/code is needed
- A contributor's documentation contribution is up for review

**When NOT to use:** evaluating prose style, tone, or readability (writing-quality review, not a structural audit); auditing the design narrative within a single design doc (that's `design-doc-review`'s domain); cross-checking against external systems beyond scope (out of graybox boundary).

## Smell Profile

Named categories used as report vocabulary:

- **Spec / code drift** — doc claims a behavior, signature, field, or flow that contradicts the in-scope spec or code
- **Stale information** — version numbers, dates, deprecated entities, or removed components persist in the doc
- **ADR rationale gap** — Architecture Decision Record states the decision without alternatives considered, trade-offs, or rejection reasons
- **Quickstart / install gap** — getting-started path is missing, incomplete, or broken when traced step-by-step against the in-scope artifact
- **API doc gap** — public API surface lacks documentation, or doc lists methods/fields that no longer exist in the spec/code
- **Diagram-text mismatch** — diagram labels, component names, or arrows do not match the prose or the spec/code
- **Doc duplication drift** — same fact appears in multiple docs with diverging values; no single source of truth
- **Cross-link rot** — internal anchor, file link, or section reference is broken within the in-scope document set
- **Audience unclear** — doc does not state who it serves (operator, integrator, end user, contributor); content shifts audience mid-document
- **Maintenance signal absent** — doc has no last-updated marker, no owner, no review cadence, no deprecation note for older sections

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual artifact under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a doc path, section heading, or line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A documentation artifact (README, API reference, ADR, architecture overview, runbook, design doc set) along with the in-scope spec or code it documents (so drift can be checked). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the documentation and the explicitly-provided reference artifact(s) — no live system, no external link content, no third-party docs — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Doc Alignment Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths — both doc and reference artifact>
**Reviewer:** sniffer/doc-alignment-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as doc path, section, or line range.>

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

- Prose style, tone, or readability judgments
- Information-architecture judgments (what should or should not be documented)
- Cross-checks against external systems, third-party docs, or runtime telemetry
- Performance or security evaluation of the system being documented
- Content loaded from external links

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
