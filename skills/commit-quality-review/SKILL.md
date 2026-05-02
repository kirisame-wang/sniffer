---
name: commit-quality-review
description: Sniffs bad smells in git commit history (commit messages, commit atomicity, branch hygiene, attribution, sensitive content) without rewriting it. Reports the few concentrations that matter; holds full evidence in working memory. Use when a branch is up for merge, when a long-running branch needs a history pass, or when reviewing a contributor's commit series.
phase: Ship
---

# commit-quality-review

## Overview

A graybox audit skill for git commit history. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of commits and branches: atomicity, message clarity, why-vs-what, attribution, branch hygiene, sensitive content. The substance of whether the underlying code change is correct (correctness review) and whether it follows project conventions for code style (complexity review) belong to other audit domains.

## When to Use

- A feature branch is up for merge and the commit series is reviewable
- A long-running branch has accumulated commits and needs a coherence pass
- A contributor's commit series is up for review before merge

**When NOT to use:** evaluating the code change itself for correctness (correctness review, not a history audit); rewriting history through rebase or squash (operational action, not a graybox audit); auditing CI configuration referenced from commit hooks (that's `pipeline-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Commit atomicity violation** — one commit bundles unrelated changes (refactor + feature + dependency bump) without a uniting purpose, or mixes formatting/style-only edits with behavior changes so the behavior diff is buried in noise
- **Commit message vagueness** — message reads `fix`, `update`, `wip`, `more changes`, with no information content
- **Convention violation** — commit message does not follow the project's commit convention (type, scope, subject-line shape) where one is established
- **Why-vs-what gap** — message restates the diff but does not state the motivation, ticket reference, or trade-off
- **Attribution gap** — pair / mob / agent-assisted commits without co-author trailers; review or AI involvement undocumented
- **Squash candidate** — multiple consecutive WIP/fix-typo/address-review commits that represent one logical change
- **History rewrite risk** — commits indicate rebase / amend / force-push on a branch shared with others, or shared branch shows non-fast-forward history
- **Branch naming drift** — branch name does not follow project pattern (feature/, fix/, ticket-id) or carries personal/temporary identifiers
- **Conflict resolution opacity** — merge commit with no message, or commit containing unresolved conflict markers (`<<<<<<<`)
- **Sensitive content commit** — secrets (API key, password, token), large binaries, generated files, or personal data appear in tree

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual history under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a commit hash, branch name, or message line). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

Git commit history (a branch's commit list, a single commit series, a merge candidate). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers commit messages, commit metadata, file diffs visible in the history, and branch state — no live execution, no remote ref-log inspection beyond what is provided, no external links.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Commit Quality Review — <branch / series name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <branch ref, commit range>
**Reviewer:** sniffer/commit-quality-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as commit hash, branch name, or message excerpt.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All named categories were scanned. Observations beyond the top concentrations are held in working memory; any category, commit, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Correctness or design judgment on the code change itself
- Recommendations to rebase, squash, or rewrite history (these are reviewer-triggered actions, not part of the audit artifact)
- Cross-checks against issue trackers, PR comments, or CI history not provided in scope
- Performance evaluation of the changed code
- Content loaded from external links or third-party services

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
