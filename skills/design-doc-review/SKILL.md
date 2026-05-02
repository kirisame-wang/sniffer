---
name: design-doc-review
description: Sniffs bad smells in design documents (specs, ADRs, architecture overviews, RFCs) — design's standalone soundness, plus design-vs-requirements inheritance when the requirements artifact is also in scope. Reports the few concentrations that matter; holds full evidence in working memory and surfaces it on request. Use when reviewing any design document — before implementation begins, after accumulated edits need a coherence pass, or as a fast first-pass scan before a longer review session.
phase: Define
---

# design-doc-review

## Overview

A graybox audit skill for design document fidelity — standalone soundness when no requirements artifact is in scope, requirements fidelity when one is. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request — "what about X?" brings the relevant observations forward.

The canine, not the judge. The canine alerts on the strongest scent and waits for the handler to ask where else.

**Standalone categories** (design only) audit the design's structural soundness as a decision document. **Inheritance categories** (design + requirements) additionally audit whether the design correctly addresses the frozen requirements. Categories marked "(requires requirements in scope)" are skipped with an explicit Coverage note when no requirements artifact is available.

## When to Use

- A design document, spec, ADR, or RFC is up for review before implementation
- A long-running design doc has accumulated edits and needs a coherence pass
- A design and its driving requirements are both in scope and inheritance review is wanted
- You want a fast first-pass scan to focus a longer review session

**When NOT to use:** mid-implementation drift checks, code review, copy-editing, or evaluating whether a technical choice is correct.

## Smell Profile

Named categories used as report vocabulary:

**Standalone categories** (apply whenever design is in scope):

- **Completeness gaps** — a standard section is missing (Objective / Success Criteria / Boundaries / Open Questions / Alternatives Considered / Consequences)
- **Undocumented decisions** — a choice is stated without Context / Alternatives / Consequences nearby
- **Unresolved decisions** — "tentative" / "TBD" / "to be confirmed" without owner or deadline
- **Internal contradictions** — same concept defined differently across sections; duplicate section numbers; stale cross-references
- **Claim-vs-structure mismatch** — the doc declares a classification but downstream structure doesn't reflect it
- **Self-application failure** — the doc defines a standard the doc itself doesn't pass
- **Paraphrase overlap** — the same idea restated across non-adjacent sections
- **Aspirational language** — strong claims with no artifact, owner, or verification path
- **Scope sprawl** — a single doc serves multiple distinct reader responsibilities, causing structural mismatch beyond reviewer attention

**Inheritance categories** (require requirements artifact in scope; skipped with explicit Coverage note when absent):

- **Requirements coverage gap** — some requirements have no design treatment, decision, or component addressing them; or design adds out-of-requirements features without an explicit re-opening / escalation note
- **Requirements traceability gap** — design decisions / non-functional translations / constraint propagations do not link to specific requirements; the why-trail from design to requirement is unreachable; requirements' NFRs and constraints are not surfaced as measurable design choices

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual document, not a pre-authored combination. Each observation carries a category tag and an evidence anchor. Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

One or more design documents (`*.md`, `*.rst`, `*.txt`, HTML) — **required**. Optionally, the requirements artifact the design realizes (PRD, functional spec, BRD, user-story collection) — when this is in scope, inheritance categories are evaluated; when absent, those categories are reported as skipped in the Coverage section. Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the in-scope document(s) only — no source code, issue trackers, runtime data, or external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Design Review — <doc name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/design-doc-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as §X:N or §X:N-M (multiple anchors comma-separated).>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All in-scope categories were scanned. Observations beyond the top concentrations are held in working memory; any category, location, or finding is expandable on request (e.g. `expand contradictions`, `what's near Section 8?`).

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*

*Skipped (requires upstream artifact in scope): <list inheritance categories whose upstream artifact is not in scope; omit this line when all categories ran>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Judgments on design quality, soundness, or technical choices
- Fix recommendations, refactor suggestions, priorities, or grades
- Cross-checks against source code
- Corrected typos or grammar (locations only — not corrections)
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request. The audit's job is to keep the *artifact* judgment-free; the agent remains unmuzzled in chat.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
