---
name: test-coverage-review
description: Sniffs bad smells in test suites and test plans (unit, integration, end-to-end test files; test plan documents) without opening the box. Reports the few concentrations that matter; holds full evidence in working memory. Use when a test suite is up for review before merge, when a long-running suite has accumulated drift, or when reviewing a contributor's tests.
phase: Verify
---

# test-coverage-review

## Overview

A graybox audit skill for test suites and test plans. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of test coverage and test design: branch coverage, boundary cases, assertion strength, isolation, naming. The substance of whether the right behavior is being tested (test strategy judgment) and whether tests pass against current code (execution result) belong to other audit domains.

## When to Use

- A test suite or test plan is up for review before merge
- A long-running suite has accumulated tests and needs a coherence pass
- A contributor's test artifact is up for review before merge

**When NOT to use:** debating test strategy or framework choice (positioning conversation, not an audit); checking whether tests pass on the current code (that's an execution run, not a graybox audit); auditing source code under test (that's `complexity-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Error path coverage gap** — error branches, exception handlers, or failure modes have no test
- **Happy-path-only coverage** — test set covers nominal inputs but no negative, boundary, or adversarial cases
- **Boundary case absence** — edge inputs (empty, null, max, min, off-by-one) untested for code that explicitly handles them
- **Weak assertion** — test asserts only that "no error was thrown" or checks output truthiness without verifying value or shape
- **Test name vagueness** — names like `test_works`, `test_case_1`, `it should be ok` give no signal about what behavior is verified
- **Setup coupling** — tests share mutable setup such that order or skipping changes outcomes
- **Test interdependence** — one test depends on side effects from another; tests cannot run in isolation
- **Mock over-reliance** — tests mock the unit under test or so much of its environment that the assertion verifies the mock, not the code
- **Regression coverage gap** — known-bug fix lands without a test that fails before the fix and passes after
- **Flaky test tolerance** — tests marked skip / retry / xfail without a tracked reason or expiry

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual test artifact, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a test file path, a test name, a describe/context block, a line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A test artifact (test files in any language, test plan document, coverage report companion). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the test artifact(s) only — no source code under test, no live test execution, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Test Coverage Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/test-coverage-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as test path, test name, or line range.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All named categories were scanned. Observations beyond the top concentrations are held in working memory; any category, test, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Test strategy or framework recommendations
- Recommendations to add, remove, or restructure tests
- Cross-checks against the source code under test
- Live test execution results, coverage percentage computation, or runtime timing
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
