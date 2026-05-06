---
name: skill-design-review
description: Audits SKILL.md files for sniffer-specific convention conformance and self-validation failures — frontmatter, boundary anchors, HOW prescription, coupling, and skill-local file quality. Reports top concentrations; holds full evidence in working memory. Use before merge, during revision, or as the DoD self-audit gate.
phase: Review
---

# skill-design-review

## Overview

A graybox audit skill for SKILL.md files. Across the SKILL.md and any skill-local supporting files, the complete observation set lives in working memory; only the concentrations that warrant the reviewer's next move surface in the artifact.

This skill audits what is specific to a SKILL.md as a sniffer artifact: frontmatter conventions, boundary anchors, HOW-vs-WHAT prescription, cross-skill coupling, five-criteria self-validation, length, reference format, and skill-local file quality. General document smells (contradictions, paraphrase, scope sprawl) have a separate audit domain.

## When to Use

- A new SKILL.md is finalized and up for review before merge
- An existing SKILL.md is being revised; verifying it still conforms to project conventions
- The DoD self-audit gate (every v1 skill must pass this before progressing past Phase 1/2/3)
- Reviewing a community-contributed skill before accepting it

**When NOT to use:** auditing the *content* of a non-SKILL.md design doc (that's `design-doc-review`'s audit domain); judging whether the skill's *concept* is worthwhile (that's a positioning conversation, not an audit); reviewing the agent's actual *runtime behavior* when running the skill (that's a behavioral test, not a doc review).

## Smell Profile

Named categories — SKILL.md-specific. Each category covers a distinct aspect of sniffer convention conformance or self-validation quality.

- **Frontmatter convention violations** — `name`, `description`, or `phase` violates Anthropic hard rules or sniffer conventions
- **Missing boundary anchors** — `Out of Scope` or `Output Format` section absent or vague (these two sections are the core self-constraint mechanism)
- **HOW prescription** — SKILL.md teaches step-by-step methodology instead of declaring responsibility (skills declare WHAT and WHAT NOT, not HOW)
- **Cross-skill hard dependency** — SKILL.md cites a sibling skill as a required input or runtime dependency
- **Five-criteria self-validation failure** — fails interface clarity, internal encapsulation, single responsibility, independent execution, or low coupling
- **Boundary scope leak** — audit boundaries (Out of Scope, no-recommendations, no-severity) leak beyond the artifact (into follow-up chat, into form-rule sections like Verification/Red Flags)
- **Length sprawl** — SKILL.md exceeds project line limits, inlines content that belongs in skill-local supporting files, or chains references >1 layer deep
- **Reference format violations** — `## References` doesn't follow conventions, OR markdown links escape the skill's own folder
- **Sample memoir** — `bad-smell-samples.md` reads as project history rather than generic patterns
- **Sample redundancy** — multiple catalog rows describe the same generic shape with surface variations

Detailed patterns, surface signals, and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual SKILL.md, not a pre-authored combination. Each observation carries a category tag and an evidence anchor. Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

The target `SKILL.md` plus any skill-local supporting files in the same directory. Scope-ambiguous cases require reviewer confirmation rather than silent picking. Smell category rules are embedded in the Smell Profile above. Cross-reference allowed only between in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Skill Design Review — <skill name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** skills/<name>/SKILL.md (+ supporting files)
**Reviewer:** sniffer/skill-design-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as `<file>:N` or `<file>:N-M` (multiple anchors comma-separated).>

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

- Judgments on whether the skill's *concept* is worthwhile or correctly conceived
- Fix recommendations, refactor suggestions, priorities, or grades
- Generic document smells (contradictions, paraphrase, scope sprawl, completeness gaps, claim-vs-structure mismatch, aspirational language, self-application failure as a generic concept) — these are a separate audit domain
- Behavioral verification of the skill at runtime (does the skill actually work?)
- Comparison with sibling skills' design choices

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
