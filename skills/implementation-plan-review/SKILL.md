---
name: implementation-plan-review
description: Sniffs bad smells in implementation plans — plan's structural readiness for design-conformance (standalone), plus design-vs-plan inheritance when the design artifact is also in scope. Reports the few concentrations that matter; holds full evidence in working memory. Use when an implementation plan is finalized after a design sign-off, when a long-running plan needs a design-conformance pass, or when reviewing a contributor's plan submission.
phase: Plan
---

# implementation-plan-review

## Overview

A graybox audit skill for implementation plan fidelity — structural fidelity when no design artifact is in scope, design fidelity when one is. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

**Standalone categories** (plan only) audit the plan's structural readiness independent of any upstream design — does it state its goal, bound its scope, assign ownership, surface unresolved items, and scaffold the structural slots needed to carry design-conformance? **Inheritance categories** (plan + design) additionally audit whether each design element is correctly carried into the plan, whether the plan references the design at all, and whether constraints are encoded to prevent drift. Categories marked "(requires design in scope)" are skipped with an explicit Coverage note when no design artifact is available.

The substance of whether the task list itself is structurally sound (atomicity, sequencing, vertical slicing — `task-decomposition-review`'s domain), whether the design itself is coherent (`design-doc-review`'s domain), and whether the requirements that drove the design are clear (`requirements-doc-review`'s domain) belong to other audit domains. Same artifact (a task list / plan document) can be reviewed by both `task-decomposition-review` and this skill from different angles — multi-perspective observation is expected.

## When to Use

- An implementation plan is finalized after the design has been signed off
- A long-running plan needs a design-conformance pass before kickoff
- A contributor's implementation plan is up for review against a referenced design
- A design has just been approved and the planning artifact is being prepared
- A plan is being prepared without a sign-off design yet, and the reviewer wants to verify the plan is structurally ready to carry design-conformance once design is approved (standalone mode)

**When NOT to use:** auditing the task list's structural quality independently of design (atomicity, sequencing, vertical slicing — that's `task-decomposition-review`'s domain; same plan artifact can be reviewed by both skills as multi-perspective); auditing the design narrative itself (that's `design-doc-review`'s domain); auditing whether the requirements are clear (that's `requirements-doc-review`'s domain); auditing release readiness or rollout (that's `release-readiness-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

**Standalone categories** (apply whenever a plan is in scope; check the plan's structural readiness for design-conformance regardless of whether a design artifact is also in scope):

- **Plan goal absent** — plan has no top-level objective, success condition, or "done" statement at the plan level; reader cannot tell what the overall deliverable is or when the plan as a whole is complete
- **Out-of-scope declaration absent** — plan has no explicit statement of what is NOT being implemented; scope boundary is left for implementation to discover
- **Unresolved plan items** — tasks or sections carry "TBD" / "pending" / "to be confirmed" markers without owner, resolution date, or dependency condition that closes them before dependent work begins
- **Ownership signal absent** — plan has no task-level or phase-level owner declared; reader cannot tell who is responsible for delivering or reviewing each component
- **Constraint scaffolding absent** — plan has no dedicated section, column, or template field for non-functional constraints, design-imposed limits, DoD criteria, or acceptance budgets; even if a design exists, the plan has no structural place to carry inherited constraints
- **Drift-prevention checkpoint absent** — plan has no "verify design intent is preserved" review point between groups of tasks; drift discipline is missing regardless of design content

**Inheritance categories** (require design artifact in scope; skipped with explicit Coverage note when absent):

- **Design-reference absent** — plan body / preamble / header has no link, citation, or reference to any upstream design / spec / ADR; the plan reads as if authored without any design context, and a reader cannot tell what (if anything) it is meant to realize
- **Design-task traceability gap** — tasks in the plan do not link back to design sections, ADRs, components, or interfaces; reader cannot verify which design element each task realizes
- **Design coverage gap** — design specifies N components / interfaces / decisions; only M (M < N) are represented as tasks; some design content has no execution work
- **Design constraint unencoded** — design specifies a constraint (security boundary, performance budget, data-integrity invariant, compliance rule, availability target, error-rate floor) not surfaced as a task-level acceptance criterion, DoD entry, or measurable budget; tasks describe behavior without the bound that governs it
- **Interface-locking task absent** — design specifies a component-to-component interface; no explicit task creates / freezes the contract before dependent tasks begin; consumer tasks proceed against an unlocked contract
- **Implementation latitude unbounded** — task description allows the implementer to re-open a decision the design has already made (e.g., "choose a database", "select a serialization format" when the design has decided)
- **Out-of-design task** — task is in the plan but does not trace to any design element; either the design is incomplete or the task is unauthorized scope creep
- **Design-implied sequencing violated** — design implies a foundation-first ordering (data model before API, contract before client, security boundary before feature work); plan reorders without rationale
- **Risk-mitigation task absent** — design lists known risks or open questions; plan has no corresponding mitigation, spike, or proof-of-concept task to address them before main implementation

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual plan and design under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a task ID, design section reference, line range, or constraint statement). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

An implementation plan (task list, sprint plan, milestone plan, project breakdown) — **required**. Optionally, the approved design artifact the plan realizes (design doc, ADR set, architecture overview, interface specifications) — when this is in scope, inheritance categories are evaluated; when absent, those categories are reported as skipped in the Coverage section, and only the standalone categories run. Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the in-scope plan and design only — no source code, no live execution, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Implementation Plan Review — <plan name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <plan path; design path>
**Reviewer:** sniffer/implementation-plan-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as task ID, design section, or line range.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All in-scope categories were scanned. Observations beyond the top concentrations are held in working memory; any category, location, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*

*Skipped (requires upstream artifact in scope): <list inheritance categories whose upstream design is not in scope; omit this line when all categories ran>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Structural quality of the task list independent of design (atomicity, sequencing within plan, vertical slicing)
- The design's own internal soundness or completeness
- The requirements behind the design
- Effort estimation calibration
- Recommendations to add, remove, or reshape tasks beyond surfacing the smell
- Cross-checks against source code or runtime systems
- Content loaded from external links

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
