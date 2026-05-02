# Bad Smell Samples — requirements-doc-review

Catalog of noteworthy patterns this skill recognizes for requirements-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a section heading, requirement ID, line range, or table row). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual document under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Requirement reads "The system shall provide a dashboard" with no `as <user>, I want <capability>, so that <outcome>` framing or equivalent goal narrative | User story structure gap |
| Document is a flat bullet list of features ("login", "search", "export") without per-item user motivation or outcome | User story structure gap |
| Requirements reference "the user" / "users" generically; no persona section, no user-segment definitions, no role differentiation | Persona definition gap |
| Personas section names two personas but their needs, contexts, and constraints are identical or undifferentiated | Persona definition gap |
| System clearly involves admin, operator, partner, and regulator but only end-user requirements are listed | Stakeholder coverage gap |
| Multi-tenant or marketplace product describes only one side (buyer-only, tenant-only) with no requirements for the other side | Stakeholder coverage gap |
| Requirement says "support exporting reports" with no condition for what counts as supported (formats, filters, data scope, success threshold) | Acceptance criteria absent (requirement-level) |
| Requirement's "definition of done" is "feature works" or "users can do X" with no observable check | Acceptance criteria absent (requirement-level) |
| Requirements list 30 items with no priority indicator on any of them | Priority signal absent |
| Requirements have priorities labeled but every item is "P0" or "must-have"; no differentiation | Priority signal absent |
| Performance, security, availability, compliance bullets are mixed with functional bullets in the same list with no category separator | Functional / non-functional split unclear |
| Document has functional requirements only; no NFR section, no performance budget, no availability target, no security requirement | Functional / non-functional split unclear |
| Document lists what the product will do; no "out of scope" / "non-goals" / "deferred to later release" section | Out-of-scope unstated |
| Scope statement is "build a CRM" with no boundary on what kinds of CRM features are excluded (no marketing automation, no helpdesk, no telephony) | Out-of-scope unstated |
| Product handles healthcare data with no mention of HIPAA, BAA, or data-residency constraint | Constraint silence |
| Integration-heavy product lists no third-party SLA, no API rate-limit assumption, no supported-platform statement | Constraint silence |
| Requirement reads "the system should be fast" / "the UI should be intuitive" / "data should be secure" with no measurable definition | Ambiguous quantifier |
| Requirement reads "supports many users" / "handles large data" without a numeric target | Ambiguous quantifier |
| Requirement is stated as a goal ("improve onboarding") with no acceptance test, no metric, no demo criterion, no measurement plan | Verification path absent |
| Requirement promises a behavior with no testable form — reviewer cannot determine how to verify it before or after implementation | Verification path absent |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
