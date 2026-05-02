---
name: ui-quality-review
description: Sniffs bad smells in UI specifications and mockups (Figma exports, design docs with screens, component specs, style guides) without opening the box. Reports the few concentrations that matter; holds full evidence in working memory. Use when a UI spec is finalized before implementation, when a long-running design has accumulated screens, or when reviewing a contributor's UI artifact.
phase: Build
---

# ui-quality-review

## Overview

A graybox audit skill for UI specifications and mockups. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a UI spec: state coverage, accessibility, responsive behavior, interaction feedback, component coherence. The substance of whether the chosen visual direction is the right one (design taste judgment) and whether the implementation matches the mockup (drift detection) belong to other audit domains.

## When to Use

- A new UI spec or mockup is finalized before implementation kickoff
- A long-running design has accumulated screens and needs a coherence pass
- A contributor's UI artifact (mockup, component spec, style guide) is up for review before merge

**When NOT to use:** evaluating brand direction or visual taste (design critique, not an audit); checking whether the built UI matches the mockup (that's `doc-alignment-review`'s domain); auditing the prose narrative of a design doc (that's `design-doc-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Accessibility gap** — alt text, ARIA roles, keyboard focus order, focus-visible state, or contrast specification absent
- **Responsive breakpoint silence** — behavior at mobile / tablet / desktop widths undefined, or only one width specified
- **State coverage gap** — loading, error, empty, skeleton, or disabled states missing for screens that fetch or mutate data
- **Interaction feedback silence** — hover, active, focus, pressed, or disabled visual specs absent for interactive elements
- **Component reuse drift** — the same conceptual element (button, card, input) appears with different specs across screens without rationale
- **Microcopy gap** — placeholder text, error messages, empty-state copy, confirmation labels not authored
- **Form validation unspecified** — input rules, error placement, inline-vs-submit timing, success feedback undefined
- **Visual hierarchy unclear** — primary / secondary / tertiary actions not differentiated, spacing or contrast rationale missing
- **Asset specification incomplete** — icons, images, illustrations lack size, format, density, or source specification
- **Internationalization silence** — no string externalization plan, no consideration for RTL languages or text-length variance

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual UI artifact, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a screen name, component name, frame reference, or section). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A UI artifact (Figma export, mockup PDF, design doc with embedded screens, component specification, style guide, or equivalent). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact document(s) only — no source code, runtime UI, or external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# UI Quality Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/ui-quality-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as screen name, component, or section reference.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All named categories were scanned. Observations beyond the top concentrations are held in working memory; any category, screen, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Judgments on visual style, brand direction, or design taste
- Recommendations to add, remove, or redesign screens
- Cross-checks against built UI or implementation code
- Performance evaluation of the rendered UI
- Content loaded from external links or third-party design systems

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
