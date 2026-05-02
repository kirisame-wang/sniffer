---
name: runtime-test-review
description: Sniffs bad smells in runtime test plans and runtime test reports (browser test recipes, manual QA scripts, exploratory session reports, runtime verification protocols) without opening the box. Reports the few concentrations that matter; holds full evidence in working memory. Use when a runtime test plan is finalized before execution, when a runtime test report is up for review, or when reviewing a contributor's runtime verification artifact.
phase: Verify
---

# runtime-test-review

## Overview

A graybox audit skill for runtime test plans and runtime test reports. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a runtime test artifact: reproduction steps, observation channels, evidence capture, environment precondition. The substance of whether the test should pass against the actual system (execution result) and whether the test design is the right strategy (test strategy judgment) belong to other audit domains.

## When to Use

- A runtime test plan is finalized before execution (browser script, QA recipe, runtime verification protocol)
- A runtime test report or exploratory session log is up for review
- A contributor's runtime verification artifact is up for review before merge

**When NOT to use:** evaluating whether a runtime test passes against the actual system (that's an execution run, not a graybox audit); auditing automated unit/integration test suites (that's `test-coverage-review`'s domain); reviewing the underlying source code (that's `complexity-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Reproduction steps incomplete** — steps skip a precondition, an action, or rely on tacit knowledge to reproduce
- **State precondition silence** — actor setup (which user, role, or auth mechanism) or data setup (seeded records, browser cache) is undefined
- **Observation channel gap** — the plan exercises a flow but does not state what is watched (console, network, DOM, storage, performance metrics)
- **Evidence capture gap** — claims of pass/fail with no screenshot, log excerpt, HAR, trace, or recorded artifact
- **Variant coverage gap** — only one browser, viewport, network condition, or locale exercised when the system supports many
- **Failure-mode probe absence** — only nominal flow traversed; offline, throttled, server-error, permission-denied paths absent
- **Cleanup specification silence** — no spec for restoring state after the test (signing out, clearing storage, reverting fixtures)
- **Timing and ordering ambiguity** — async actions sequenced without wait conditions, timing tolerances, or ordering markers
- **Environmental assumption silence** — system configuration (feature flag, locale, time-zone, clock) left implicit and not stated in the test plan
- **Result interpretation silence** — pass / fail / inconclusive declared without criteria; what counts as a regression is undefined

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual test artifact, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a step number, a section, a scenario name, a line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A runtime test artifact (browser test recipe, manual QA script, exploratory session report, runtime verification protocol, runbook for runtime checks). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact document(s) only — no live system, no recorded video, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Runtime Test Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/runtime-test-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as step number, scenario, or section reference.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All named categories were scanned. Observations beyond the top concentrations are held in working memory; any category, scenario, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Test strategy or framework recommendations
- Recommendations to add, remove, or redesign scenarios
- Live execution of the runtime test against any system
- Performance evaluation of the system under test
- Content loaded from external links or third-party tooling docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
