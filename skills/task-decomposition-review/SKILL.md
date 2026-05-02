---
name: task-decomposition-review
description: Sniffs bad smells in task decomposition plans (sprint backlogs, feature breakdowns, implementation task lists) without opening the box. Reports the few concentrations that matter; holds full evidence in working memory. Use when a task breakdown is finalized before implementation kickoff, when a task list has accumulated edits and needs a coherence pass, or when reviewing a contributor's planning artifact.
phase: Plan
---

# task-decomposition-review

## Overview

A graybox audit skill for task decomposition plans. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request — "what about §3?" brings the relevant observations forward.

This skill audits the structural quality of a task plan: atomicity, verifiability, sequencing, dependencies. The substance of whether the right tasks are listed (product judgment) or whether estimates are correct (calibration judgment) belongs to other audit domains.

## When to Use

- A new sprint backlog or feature plan is finalized before implementation kickoff
- A long-running task list has accumulated edits and needs a coherence pass
- A contributor's planning artifact is up for review before merge

**When NOT to use:** project-level roadmaps and positioning docs (that's `design-doc-review`'s audit domain); reviewing the merits of which tasks to include (that's a product conversation, not an audit); calibrating effort estimates (that's a project judgment).

## Smell Profile

Named categories used as report vocabulary:

- **Atomicity violations** — a task is too large (spans multiple independent deliverables with no single "done" boundary) or too trivial (outcome too small to close meaningfully), regardless of whether the concerns are homogeneous
- **Acceptance criteria gaps** — a task has no explicit "done" condition, or describes work without a verifiable outcome
- **Verification path silence** — a task lacks a check (test, manual confirmation, observable output) that closes it; or the plan has no phase-level checkpoint that confirms the system is in a working state between groups of tasks
- **Hidden dependencies** — a task waits on another but the dependency is implicit, or the critical path is obscured by ordering alone
- **Sequencing problems** — tasks ordered without rationale; foundation tasks deferred behind feature tasks; refactor and feature tasks interleaved
- **Vertical-slice absence** — plan groups work by horizontal layer (all-DB → all-API → all-UI) so no task delivers an end-to-end working path on its own
- **Scope bundling** — a single task mixes heterogeneous work categories (e.g., refactor + feature + docs, or backend + infra + copywriting) even when the total size would be reasonable for one task
- **Effort uncalibrated** — no effort indicator on tasks, or estimates wildly inconsistent across similar tasks without explanation
- **Orphaned tasks** — a task listed without parent goal, owner, or rationale linking it to the spec
- **Premature optimization in plan** — performance, scaling, or generalization tasks scheduled before the core MVP path is verified
- **Risk register silence** — plan has no risks/mitigations enumeration, no open-questions section, no acknowledgment of unknowns or external blockers

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual plan, not a pre-authored combination. Each observation carries a category tag and an evidence anchor. Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A task decomposition document or list (sprint backlog, feature breakdown, implementation plan, GitHub issues collection exported as markdown). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the plan document(s) only — no source code, issue tracker live state, runtime data, or external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Task Decomposition Review — <plan name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/task-decomposition-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as task ID, line, or section anchor.>

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

- Judgments on whether the chosen tasks are the right tasks for the goal (product decision)
- Calibration of effort estimates against historical velocity (project decision)
- Recommendations to add, remove, or reorder specific tasks
- Cross-checks against source code or issue tracker live state
- Content loaded from external links, third-party tools, or runtime systems

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
