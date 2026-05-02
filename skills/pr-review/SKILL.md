---
name: pr-review
description: Sniffs bad smells in pull requests / merge requests as merge-handoff artifacts (PR description, linked issues, test plan, reviewer assignment, evidence, breaking-change marking, merge-readiness signals) without merging them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a PR is opened for review, when a long-running PR has accumulated commits and discussion, or when reviewing a contributor's PR submission.
phase: Ship
---

# pr-review

## Overview

A graybox audit skill for pull requests as merge-handoff artifacts. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits whether a PR is review-ready and well-formed for a clean merge: description completeness, linkage to motivating context, test plan, scope coherence, reviewer coverage, pre-merge state, breaking-change signaling, evidence, merge-strategy alignment with project policy. The substance of whether the code change itself is correct (per-domain code review), whether the git commit history is clean (`commit-quality-review`'s domain), whether the CI pipeline configuration is sound (`pipeline-review`'s domain), and whether the change should ship strategically (`release-readiness-review`'s domain) belong to other audit domains.

This audit complements rather than replaces configured PR-handling tooling (CODEOWNERS, branch protection rules, required-status-checks, PR template enforcement, stale-bot, Conventional-Commits / semantic-release linters). Many concerns here are semantic — description quality, test-plan adequacy, evidence sufficiency, scope coherence — that tools cannot enforce until rules are explicitly authored, and several that tools cannot enforce at all. The audit is most valuable when no such tooling is configured, or as a complement to tooling that catches only declared violations.

## When to Use

- A PR is opened and marked ready for review
- A long-running PR has accumulated commits and review discussion and needs a coherence pass before merge
- A contributor's first PR or an unfamiliar-area PR is up for review
- A PR is being prepared and the author wants a self-check before requesting review

**When NOT to use:** auditing the correctness of the code change itself (delegate to per-domain skills such as `complexity-review`, `security-review`, `architecture-review`); auditing the git commit history within the PR (that's `commit-quality-review`'s domain); auditing CI / CD pipeline configuration (that's `pipeline-review`'s domain); evaluating whether the change should ship at all (that's `release-readiness-review`'s domain); evaluating prose style or readability of the PR body (writing-quality concern, not a structural audit).

## Smell Profile

Named categories used as report vocabulary:

- **PR description vagueness** — body is empty, a single line, or mechanically restates the title; no information beyond what the diff already shows
- **Why-vs-what gap** — body lists the changes without stating motivation, business context, or trade-off considered
- **Linked issue absence** — no link to the ticket, issue, RFC, design doc, or originating discussion that motivated the change; the why-trail is unreachable
- **Test plan absence** — body lacks a "how this was tested / how to verify" section, or it reads "tested manually" / "covered by existing tests" with no specifics
- **Scope mismatch** — title/description describes one focused change but diff bundles unrelated work (refactor + feature + dependency bump + style); PR-level atomicity violated
- **Reviewer assignment gap** — no reviewer assigned where policy requires peer review; assigned reviewer owns no portion of the touched files; CODEOWNERS coverage of touched paths is missing
- **Pre-merge cleanup risk** — WIP / fix-typo / address-review commits remain unsquashed when policy requires clean history; review threads remain unresolved at merge; required CI checks bypassed via admin override
- **Breaking change unmarked** — PR introduces semver-breaking change, schema migration, or downstream-coordination need with no breaking-change label, no version-bump note, no migration callout
- **Evidence absence** — UI changes have no screenshot / recording; performance claims have no benchmark numbers; bug fix has no failing-then-passing test result or reproduction trace
- **Merge-strategy / template mismatch** — chosen merge strategy contradicts project policy without rationale; PR template required boxes left unchecked with no explanation; PR uses a stale template missing current required sections

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual PR under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a PR section, file path, commit reference, review-thread URL, or CI-status row). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A pull request / merge request artifact: PR body, title, labels, linked issues, reviewer assignments, commits referenced by the PR, review threads, CI status visible in the PR view, and the project's PR template / CODEOWNERS / branch-protection policy when those are in the in-scope set. Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers what's visible in the PR view — no live deployment, no production state inspection, no external link content beyond what the in-scope set explicitly includes.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# PR Review — <PR title or number>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <PR URL or identifier; in-scope policy artifacts if any>
**Reviewer:** sniffer/pr-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as PR section, commit reference, review-thread anchor, or CI-status row.>

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

- Correctness or quality of the code change itself
- Git commit history audit within the PR
- CI / CD pipeline configuration audit
- Strategic judgment on whether the change should ship
- Prose style, tone, or readability of the PR body
- Live deployment, production state inspection, or post-merge metric collection
- Recommendations to add, remove, or reshape PR content beyond surfacing the smell
- Content loaded from external links beyond the explicit in-scope set

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
