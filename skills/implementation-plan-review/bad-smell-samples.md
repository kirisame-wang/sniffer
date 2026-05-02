# Bad Smell Samples — implementation-plan-review

Catalog of noteworthy patterns this skill recognizes for plan structural smells (standalone) and design-to-plan inheritance smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a task ID, design section reference, constraint statement, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual plan and design under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Plan body / preamble / header has no link, citation, or reference to any upstream design / spec / ADR; reader cannot tell what (if anything) the plan is meant to implement | Design-reference absent |
| Plan body mentions "see the auth design doc" or "per the ADR" in prose but contains no parseable link, path, or identifier; reader cannot navigate to the upstream artifact | Design-reference absent |
| Plan opens directly with a task list; no "Goal:", "Objective:", or "This plan delivers…" statement at the top; reader cannot tell what "done" means for the whole effort | Plan goal absent |
| Plan has a title ("Q3 Auth Refactor") but no success condition, acceptance milestone, or definition of complete at plan level | Plan goal absent |
| Plan lists tasks for authentication and authorization with no "out of scope" or "not in this plan" section; reader cannot tell if rate limiting, session management, or SSO are deferred | Out-of-scope declaration absent |
| Plan has an "Out of scope" section but the body is empty or reads "TBD" — the structural slot exists but carries no exclusion signal | Out-of-scope declaration absent |
| Task reads "Implement caching layer (approach TBD)" with no owner, no resolution date, and no blocking dependency that closes the TBD before dependent tasks begin | Unresolved plan items |
| Plan section is marked "PENDING design sign-off" with no due date and no blocking relationship declared; downstream tasks proceed as if it were resolved | Unresolved plan items |
| Plan lists 20 tasks across backend, frontend, and infrastructure with no owner on any task; reader cannot tell who is accountable for any deliverable | Ownership signal absent |
| Plan has phase checkpoints ("Phase 1 complete", "Phase 2 complete") with no reviewer or sign-off owner named | Ownership signal absent |
| Plan has no constraint section, no DoD column, no acceptance-criteria field, no NFR slot — just task titles and descriptions; plan structurally cannot carry design-imposed constraints | Constraint scaffolding absent |
| Plan has a tasks table with Description and Status columns; constraints and acceptance criteria are embedded in Description cells as prose rather than captured in a dedicated field | Constraint scaffolding absent |
| Plan lists 30 tasks in sequence with no checkpoint that asks "does the implementation so far still match the design?" | Drift-prevention checkpoint absent |
| Plan ends at "implementation complete" with no design-conformance review step before declaring done | Drift-prevention checkpoint absent |
| Tasks in the plan have no link, citation, or annotation pointing back to the design section, ADR, component, or interface they realize | Design-task traceability gap |
| Plan introduces tasks under generic phase headings ("backend work", "frontend work") with no per-task design reference | Design-task traceability gap |
| Design specifies five components (auth, billing, notification, search, reporting); plan tasks cover three of them; two have no execution work | Design coverage gap |
| Design lists open questions or decisions to make during implementation; plan does not enumerate them as tasks | Design coverage gap |
| Design specifies a security boundary (e.g., "all admin endpoints require step-up MFA") or data-integrity invariant; no task carries this as an acceptance criterion or DoD entry | Design constraint unencoded |
| Design specifies "p95 latency under 200ms" / "99.9% availability" / compliance rules; tasks describe what to build with no measurable budget or acceptance statement | Design constraint unencoded |
| Design defines an API contract / IPC schema / event format between components; no task creates or freezes that contract before consumer tasks start | Interface-locking task absent |
| Plan parallelizes producer and consumer tasks against an interface that has not been explicitly locked in an earlier task | Interface-locking task absent |
| Task description says "choose a queue technology" / "decide on serialization format" when the design has already specified RabbitMQ / Protobuf | Implementation latitude unbounded |
| Task allows multiple incompatible interpretations of the design (e.g., "implement caching" with no cache strategy locked from design) | Implementation latitude unbounded |
| Plan includes "migrate to Kubernetes" or "add Redis caching layer"; neither appears in the design document, no ADR, no interface spec covers it | Out-of-design task |
| "Add admin dashboard" appears in the plan; design has no admin module specified | Out-of-design task |
| Design implies foundation-first ordering (data model → API → client; contract → producer → consumer); plan schedules dependent tasks before foundation tasks without rationale (violation detectable from architectural convention, no explicit ordering statement required) | Design-implied sequencing violated |
| Design explicitly states a security boundary or migration order; plan reorders without acknowledgment of the stated ordering implication | Design-implied sequencing violated |
| Design lists three identified risks (third-party SLA, data-volume scaling, regulatory uncertainty); plan has no spike, mitigation task, or proof-of-concept addressing them | Risk-mitigation task absent |
| Design has open questions that block other decisions; plan has no early task to resolve them before dependent work begins | Risk-mitigation task absent |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
