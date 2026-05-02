---
name: doc-hygiene-review
description: Sniffs bad smells in documentation hygiene — the maintenance state of a doc set as a long-lived artifact (cross-doc fact consistency, internal-link integrity, audience clarity, lifecycle signals). Reports the few concentrations that matter; holds full evidence in working memory. Use when a doc set is up for review for hygiene-only concerns, when a long-running doc set has accumulated maintenance debt, or when reviewing a contributor's documentation contribution for hygiene.
phase: Review
---

# doc-hygiene-review

## Overview

A graybox audit skill for documentation hygiene — whether a doc set is in a state to be trusted, navigated, and maintained as a long-lived artifact. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the doc set's hygiene as a documentation set: cross-doc fact consistency, internal-link integrity, audience clarity, and presence of lifecycle signals (last-updated marker, owner, review cadence). The substance of whether the doc content matches the spec/code it describes (`doc-alignment-review`'s domain) and whether design narrative is structurally sound as a decision document (`design-doc-review`'s domain) belong to other audit domains. The audit's question is intrinsically about the doc set itself — no spec or code reference is needed or used.

## When to Use

- A documentation set is up for review and the concern is doc-set health (links, consistency, audience, lifecycle), not content-vs-source alignment
- A long-running doc set has accumulated maintenance debt (broken links, diverging facts, missing owners, stale audience signals)
- A contributor's documentation contribution is up for review for hygiene-only concerns
- A doc set is being inherited by a new owner and a baseline state assessment is needed

**When NOT to use:** auditing whether doc content matches the spec or code it describes (that's `doc-alignment-review`'s domain); auditing the design narrative, decision rationale, or ADR completeness (that's `design-doc-review`'s domain); evaluating prose style, tone, or readability (writing-quality concern, not a structural hygiene audit); evaluating whether the doc set covers the right content at the philosophical level (information-architecture judgment).

## Smell Profile

Named categories used as report vocabulary:

- **Doc duplication drift** — same fact (version requirement, configuration default, command syntax, policy statement) appears in multiple docs with diverging values; no single source of truth across the doc set
- **Cross-link rot** — internal anchor, file link, or section reference is broken within the in-scope document set
- **Audience unclear** — doc does not state who it serves (operator, integrator, end user, contributor); content shifts audience mid-document with no signpost
- **Maintenance signal absent** — doc has no last-updated marker, no owner, no review cadence, no deprecation note for older sections; reader cannot gauge whether the doc is still trustworthy

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual doc set under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a doc path, section heading, or line range). Anything that fits none of the named categories is held under "miscellaneous".

The 4-category vocabulary is narrower than sniffer's typical 8-10. This is intentional — the audit's niche is the maintenance state of a doc set, a focused domain whose smells cluster around four hygiene axes (cross-doc consistency, link integrity, audience clarity, lifecycle metadata). Padding to 8-10 would smuggle in concerns from neighboring audit questions.

## Input Contract

A documentation set (one or more docs that share a readership: README + CONTRIBUTING + API reference + ADRs, or a `docs/` tree). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the in-scope doc set only — no source code, no live system, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Doc Hygiene Review — <doc set name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/doc-hygiene-review

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

- Whether doc content matches the spec or code it describes
- Design narrative, decision rationale, or ADR completeness
- Prose style, tone, or readability
- Information-architecture judgments (what should or should not be documented)
- Cross-checks against external systems, third-party docs, or runtime telemetry
- Performance or security evaluation of the system being documented
- Content loaded from external links

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
