---
name: requirements-doc-review
description: Sniffs bad smells in requirements documents (PRD, functional spec, BRD, user-story collection, requirements section of a larger doc) without authoring them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a requirements artifact is finalized before design kickoff, when a long-running requirements doc has accumulated edits, or when reviewing a contributor's requirements contribution.
phase: Define
---

# requirements-doc-review

## Overview

A graybox audit skill for requirements documents. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits whether requirements are clear, complete, and verifiable enough to drive design — the problem-space side of the Define phase. The substance of whether the proposed solution correctly addresses these requirements (`design-doc-review`'s domain), whether the implementation plan correctly inherits from the design (`implementation-plan-review`'s domain), and whether the requirements are the right requirements at all (product judgment) belong to other audit domains.

This skill is at the head of the SDLC document chain — there is no upstream stage artifact for inheritance audit, so all categories audit the requirements document on its own merits.

## When to Use

- A requirements artifact (PRD, functional spec, BRD) is finalized before design kickoff
- A long-running requirements doc has accumulated edits and needs a clarity pass
- A user-story collection or backlog is up for review before being handed to design
- A contributor's requirements contribution is up for review before merge

**When NOT to use:** auditing the design narrative or solution decisions (that's `design-doc-review`'s domain); auditing whether the implementation plan inherits correctly from approved design (that's `implementation-plan-review`'s domain); auditing structural quality of a task list (that's `task-decomposition-review`'s domain); evaluating whether the requirements are the right product calls (positioning conversation, not an audit); evaluating prose style or readability (writing-quality concern).

## Smell Profile

Named categories used as report vocabulary:

- **User story structure gap** — requirement stated as a system feature without user-goal framing (`as <user>, I want <capability>, so that <outcome>` or equivalent); reader cannot infer who benefits or why this matters
- **Persona definition gap** — requirements reference "the user" generically with no persona profile, no user segments, no role differentiation (end-user / admin / operator / integrator)
- **Stakeholder coverage gap** — requirements cover only one stakeholder perspective when the system clearly serves multiple (admin / operator / partner / regulator absent despite obvious involvement)
- **Acceptance criteria absent (requirement-level)** — requirement stated as need without observable success condition; reader cannot determine "we know this requirement is satisfied when ..."
- **Priority signal absent** — no MoSCoW / RICE / P0-P1-P2 / must-should-could-won't classification; reader cannot tell critical from optional
- **Functional / non-functional split unclear** — performance, security, availability, compliance requirements buried within functional descriptions, or the non-functional category is missing entirely
- **Out-of-scope unstated** — what the system is NOT going to do is undeclared; scope ambiguity is left for design to discover
- **Constraint silence** — regulatory, contractual, technical, or business constraints (legal hold, data residency, third-party SLA, supported platforms) unstated; design will discover them late
- **Ambiguous quantifier** — requirements use "fast", "easy", "secure", "scalable", "many" without numeric threshold or measurable definition
- **Verification path absent** — requirement has no testable form: no acceptance test sketch, no measurement plan, no demo criterion that closes it

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual document, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a section heading, requirement ID, line range, or table row). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A requirements artifact (PRD, functional specification, BRD, user-story collection, the requirements section of a design doc when carved out). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the requirements document(s) only — no source code, no live system, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Requirements Doc Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/requirements-doc-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as section, requirement ID, or line range.>

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

- Solution-side judgments (whether a proposed design addresses the requirements)
- Implementation-plan judgments (whether tasks correctly inherit from design)
- Product-direction judgments (whether the listed requirements are the right ones)
- Prose style, tone, or readability
- Cross-checks against source code, runtime data, or external systems
- Content loaded from external links

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
