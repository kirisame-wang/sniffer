---
name: diagnosability-review
description: Sniffs bad smells in diagnosability specifications (logging plans, error handling docs, observability specs, runbooks, incident postmortems, error catalogs) without opening the box. Reports the few concentrations that matter; holds full evidence in working memory. Use when an observability spec is finalized before implementation, when a runbook is up for review, or when reviewing a contributor's diagnosability artifact.
phase: Verify
---

# diagnosability-review

## Overview

A graybox audit skill for diagnosability artifacts — documents that describe how a system is observed, logged, and recovered from failure. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a diagnosability spec: log message specificity, error context propagation, correlation, alerting, runbook completeness. The substance of whether the chosen observability stack is the right one (tooling judgment) and whether the implementation conforms to the spec (drift detection) belong to other audit domains.

## When to Use

- An observability or logging specification is finalized before implementation
- A runbook, incident postmortem, or error catalog is up for review
- A contributor's diagnosability artifact is up for review before merge

**When NOT to use:** evaluating observability tool choice (positioning conversation, not an audit); checking whether the runtime system actually emits the spec'd telemetry (that's `doc-alignment-review`'s domain); auditing the source code that emits the logs (that's `complexity-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Log message vagueness** — message templates lack context fields (operation, identifier, input shape) that make a failure diagnosable
- **Error context loss** — exceptions caught and re-raised with new message stripping cause, original stack, or operation context
- **Correlation gap** — no request ID, trace ID, session ID, or causal marker linking related log lines across services
- **Sensitive data exposure risk** — log specs include PII, secrets, tokens, or full request bodies without redaction rule
- **Log level miscalibration** — error states logged as INFO/DEBUG, or routine events logged as ERROR/WARN, distorting alert signal
- **User-facing error opacity** — error returned to caller or end user is opaque ("Internal error", "Something went wrong") without an error code, support ID, or diagnostic hook
- **Reproduction context gap** — error report or postmortem does not capture inputs, environment, version, or sequence enough to reproduce
- **Metric and dashboard absence** — critical path or SLO has no documented metric, dashboard, or saved query
- **Alerting silence** — no alert rule documented for error spikes, latency regressions, or saturation on the critical path
- **Runbook gap** — known error class or alert has no diagnostic procedure, escalation path, or remediation step documented

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual diagnosability artifact, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a log statement, an error class, a runbook section, a metric name). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A diagnosability artifact (logging spec, error handling document, observability/instrumentation plan, runbook, incident postmortem, error catalog, or equivalent). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact document(s) only — no live telemetry, no source code emitting the logs, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Diagnosability Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/diagnosability-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as log statement, error class, runbook section, or metric name.>

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

- Tool selection judgments (which logging library, which APM vendor, which dashboard tool)
- Recommendations to add, remove, or restructure observability infrastructure
- Cross-checks against runtime telemetry or source code emitting the logs
- Performance evaluation of the observability stack itself
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
