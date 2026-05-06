---
description: Sniff bad smells in Verify-phase artifacts (test suites, runtime test plans, observability material, runbooks, postmortems). Returns one unified graybox report.
argument-hint: <artifact-path>...
---

Targets: $ARGUMENTS

Sniff the supplied target(s) as Verify-phase artifacts. Engage the **Verify-phase** sniffer skills whose audit domain matches what was supplied; when more than one applies to the same target or to sibling targets in scope, run all that fit.

Merge findings into a single graybox report following sniffer's standard output shape:

- One **Top concentrations** list. A cluster may converge smells from multiple skills — tag each smell as `<source-skill>/<category>` using each contributing skill's own category vocabulary.
- One **Coverage** section listing which sniffer audit domains ran and which were skipped, with reason (e.g. *no upstream artifact in scope*, *target shape did not match this domain*).
- Sniffer's graybox boundaries hold across the merged artifact: **locate, not expand. Summary, not verdict.** No fix recommendations, scores, or priorities in the report.

After the report lands, follow-up conversation is normal interaction — opinions, expansions, or fix suggestions are available on request.
