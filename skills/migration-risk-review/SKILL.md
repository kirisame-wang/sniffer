---
name: migration-risk-review
description: Sniffs bad smells in migration and deprecation plans (data migrations, schema changes, API deprecations, rollout/rollback specs, dual-write transitions) without executing them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a migration plan is finalized, when a deprecation document is up for review, or when reviewing a contributor's migration artifact.
phase: Ship
---

# migration-risk-review

## Overview

A graybox audit skill for migration and deprecation plans. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a migration plan: backward compatibility, rollback path, data integrity, phased rollout, deprecation timeline, communication, verification, idempotency, ordering. The substance of whether the migration is the right strategic move (positioning judgment) and whether it actually executed correctly (post-migration verification result) belong to other audit domains.

## When to Use

- A migration plan (data, schema, API, infrastructure) is finalized before execution
- A deprecation document is up for review before announcement
- A contributor's migration or transition artifact is up for review

**When NOT to use:** evaluating whether a migration is the right strategic choice (positioning conversation, not an audit); checking whether a migration succeeded after running it (that's an execution review, not a graybox audit); auditing the source code that performs the migration (that's `complexity-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Backward compatibility silence** — the fate of existing consumers, old clients, or in-flight data during the transition is undefined
- **Replacement readiness gap** — deprecation announced without a working alternative that covers the existing use cases, or the replacement lacks production-maturity evidence (parity tests, traffic in prod, feature coverage statement)
- **Rollback path absence** — the plan defines forward steps with no spec for reverting, no previous-version pointer, no abort criterion
- **Data integrity gap** — backfill, dual-write, copy/verify, or cutover step is incomplete; reconciliation criterion absent
- **Phased rollout absence** — plan describes a single flip with no canary, percentage rollout, feature flag, or staged scope
- **Deprecation timeline silence** — sunset date, migration window, grace period, or hard cutoff is missing for users on the old path
- **Communication plan absence** — affected callers, owners, or end users have no defined notification step, channel, or lead time
- **Verification step gap** — no acceptance criterion, sign-off, or post-migration check that confirms the migration succeeded
- **Concurrent state risk** — old and new versions live simultaneously without spec for conflicts, write-precedence, or merge rules
- **Idempotency silence** — migration script can run more than once with unspecified effect (double-apply, double-charge, orphan record)
- **Dependency ordering unclear** — multi-component migration (DB + service + client) has no explicit step ordering or "do not deploy A before B" constraint

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual plan under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a section heading, step number, table row, or line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A migration or deprecation artifact (data migration plan, schema change spec, API deprecation document, rollout plan, dual-write transition design, runbook). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact document(s) only — no live execution, no production state inspection, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Migration Risk Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/migration-risk-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as section, step, or line range.>

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

- Strategic judgment on whether the migration should happen
- Live execution of the migration or production state inspection
- Recommendations to add, remove, or restructure migration steps beyond surfacing the smell
- Performance evaluation of the migration's runtime cost
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
