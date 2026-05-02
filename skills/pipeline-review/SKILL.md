---
name: pipeline-review
description: Sniffs bad smells in CI/CD pipeline configurations (workflow files, build/deploy scripts, gate definitions, runner specs) without executing them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a pipeline config is finalized, when a long-running pipeline has accumulated stages, or when reviewing a contributor's pipeline change.
phase: Ship
---

# pipeline-review

## Overview

A graybox audit skill for CI/CD pipeline configurations. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of pipeline definitions: stage coverage, quality gates, secret handling, triggering, environment parity, rollback path, permission scope. The substance of whether the pipeline actually runs (execution) and whether the choice of CI vendor is correct (tooling judgment) belong to other audit domains.

## When to Use

- A new pipeline configuration is finalized before activation
- A long-running pipeline has accumulated stages and needs a coherence pass
- A contributor's pipeline change is up for review before merge

**When NOT to use:** debugging an actual pipeline run failure (operational triage, not a graybox audit); selecting between CI vendors (positioning conversation, not an audit); auditing the application code the pipeline builds (that's `complexity-review` or `security-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Stage coverage gap** — pipeline lacks a build, test, lint, security, or artifact-publish stage that the project's gates require
- **Quality gate absence** — stages run but their result does not block merge / deploy; failures are advisory only
- **Caching efficiency silence** — no dependency cache, no layer cache, no path filter — builds always run cold or run on every push
- **Secret handling risk** — secrets passed via plaintext env, echoed in logs, scoped to all jobs, or rotated without process
- **Trigger condition unclear** — pipeline triggering rule (push / PR / schedule / manual) is unstated or inconsistent across workflows
- **Failure visibility gap** — failed run produces no notification, no status check, no PR comment, no on-call alert
- **Idempotency silence** — re-running the pipeline is unsafe (deploys twice, double-publishes artifact, leaves orphan resource) with no spec
- **Environment drift** — dev, staging, production pipelines diverge in steps or versions, no shared definition or parity statement
- **Rollback path absence** — deploy stage with no documented rollback procedure, no previous-version reference, no abort hook
- **Permission scope overbroad** — runner, service account, or token granted permissions wider than the job needs (admin token for read-only step, secrets exposed to forks)

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual pipeline under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a workflow file, job name, step, or line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A pipeline artifact (CI workflow file, CD config, build script, deploy script, runner specification, or equivalent). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the pipeline document(s) only — no live execution, no run-history inspection, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Pipeline Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/pipeline-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as workflow file, job, or line range.>

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

- CI vendor or tool selection judgments
- Live pipeline execution or run-history analysis
- Recommendations to add, remove, or restructure stages beyond surfacing the smell
- Performance evaluation of the application code the pipeline builds
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
