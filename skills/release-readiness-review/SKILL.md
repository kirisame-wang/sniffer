---
name: release-readiness-review
description: Sniffs bad smells in release readiness artifacts (launch checklists, go-live plans, rollout/rollback specs, monitoring prep, communications plans) without launching them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a release plan is finalized before launch, when a long-running release has accumulated steps, or when reviewing a contributor's launch artifact.
phase: Ship
---

# release-readiness-review

## Overview

A graybox audit skill for release readiness artifacts. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a release plan: acceptance coverage, monitoring prep, rollback prep, communication, support readiness, feature flags, capacity assumption, sign-offs, post-launch verification, ownership. The substance of whether the release should ship at all (positioning judgment) and whether it actually executed correctly (post-launch result) belong to other audit domains.

## When to Use

- A release plan or launch checklist is finalized before go-live
- A long-running release has accumulated steps and needs a coherence pass
- A contributor's launch artifact is up for review

**When NOT to use:** evaluating whether the product should launch (positioning conversation, not an audit); checking whether a release succeeded after launch (that's an execution review, not a graybox audit); auditing the underlying CI/CD pipeline (that's `pipeline-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Acceptance coverage gap** — feature listed for release without acceptance criteria, go-live verification, or definition of done
- **Monitoring prep gap** — new feature ships without dashboards, metrics, or alerts staged ahead of launch
- **Rollback prep gap** — release plan has no rollback procedure, no previous-version pointer, no abort criterion, no time-to-revert estimate
- **Communication plan absence** — affected users, customers, partners, or internal teams have no defined notification step, channel, or lead time
- **Support readiness gap** — runbooks, FAQs, support scripts, or on-call rotation not updated for the new feature
- **Feature flag plan unclear** — flag default, ramp schedule, kill switch, target audience, owner, or expiry/cleanup criterion undefined; flag-debt risk if no removal plan after full rollout
- **Capacity assumption silence** — anticipated load, traffic shape, or headroom under launch is not stated; capacity check absent
- **Sign-off gap** — required reviews (security, legal, compliance, accessibility, privacy) are unmarked or pending without owner
- **Post-launch verification gap** — no defined success criteria, no smoke tests scheduled, no metric thresholds, no investigation plan
- **Ownership unclear** — go / no-go decision-maker, escalation path, or on-call owner during launch is undefined

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual plan under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a section heading, checklist item, table row, or line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A release readiness artifact (launch checklist, go-live plan, rollout specification, communications plan, support handoff document, or equivalent). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact document(s) only — no live deployment, no production state inspection, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Release Readiness Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/release-readiness-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as section, checklist item, or line range.>

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

- Strategic judgment on whether the release should ship
- Live deployment, production state inspection, or post-launch metric collection
- Recommendations to add, remove, or restructure release steps beyond surfacing the smell
- Performance evaluation of the launching feature
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
