---
name: performance-review
description: Sniffs bad smells in performance artifacts and source code (performance budgets, capacity plans, benchmark specs, profiling reports, performance-sensitive modules) without running them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a performance spec is finalized, when a performance-sensitive module is up for review, or when reviewing a contributor's performance artifact.
phase: Review
---

# performance-review

## Overview

A graybox audit skill for performance-relevant artifacts. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a performance posture as documented or coded: budgets, measurement methodology, workload assumptions, algorithmic shape, caching, concurrency, regression guarding. The substance of whether the code actually meets the budget (benchmark execution) and whether the chosen target is the right one (capacity-planning judgment) belong to other audit domains.

## When to Use

- A performance budget, capacity plan, or benchmark specification is finalized
- A performance-sensitive module (hot path, batch job, large-data processor) is up for review
- A contributor's performance artifact (benchmark report, profiling write-up) is up for review

**When NOT to use:** running benchmarks or profiling against a live system (that's an execution run, not a graybox audit); evaluating product-level capacity decisions (positioning conversation, not an audit); auditing security-sensitive aspects of perf code (that's `security-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Budget absence** — no SLO, latency budget, throughput target, or memory ceiling stated for the path under review
- **Measurement methodology silence** — performance claim or benchmark with no setup spec (workload, environment, warmup, repetitions)
- **Workload assumption silence** — capacity plan or hot path silent on traffic shape (RPS, payload size, concurrency, distribution)
- **Algorithmic complexity smell** — visible O(n²) / O(n³) / quadratic-on-collection pattern in code, no rationale for input bound
- **Chatty access pattern** — query, RPC, or expensive call inside a loop over external data (classic N+1)
- **Caching specification gap** — cache used or proposed without TTL, invalidation rule, miss-path behavior, or stampede defense
- **Resource lifecycle risk** — connections, file handles, threads, goroutines, or buffers acquired without bounded close, pool, or cleanup spec
- **Concurrency design unclear** — concurrent code with no spec for contention, lock ordering, backpressure, cancellation, or deadlock avoidance
- **Premature optimization** — complex performance machinery (custom allocator, hand-rolled cache, micro-tuned loop) with no profiling evidence to justify it
- **Regression guard silence** — no performance gate, baseline metric, or alert on regression for the budgeted path

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual artifact under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a file path, function, section, or line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A performance artifact (performance budget, capacity plan, benchmark specification, profiling report) or performance-sensitive source code (hot path, batch processor, concurrent code). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact(s) only — no live benchmark execution, no profiling run, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Performance Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/performance-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as file path, section, or line range.>

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

- Live benchmark or profiling execution against any system
- Capacity-planning judgments (target RPS, instance count, scaling strategy)
- Recommendations to add, remove, or restructure performance machinery beyond surfacing the smell
- Security or threat-model evaluation
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
