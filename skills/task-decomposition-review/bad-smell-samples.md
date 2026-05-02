# Bad Smell Samples — task-decomposition-review

Catalog of noteworthy patterns this skill recognizes for task-decomposition-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a task ID, a description line, an ordering position). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual plan under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Task "Implement user authentication" bundles login, registration, password reset, OAuth integration, and 2FA — five independently deliverable flows under one ticket | Atomicity violation |
| Task list contains "fix small typo" alongside "rewrite authentication module" — two-orders-of-magnitude granularity gap without rationale | Atomicity violation |
| Task description ends at "implement the new endpoint" with no condition for being done | Acceptance criteria gap |
| Task acceptance criterion is "code is working" or similar non-verifiable phrasing | Acceptance criteria gap |
| Task has acceptance criterion but no test, manual check, or observable output that confirms it | Verification path silence |
| Plan lists 10+ tasks in sequence with no checkpoint between phases that confirms the system still builds, tests pass, or core flows work | Verification path silence |
| Task B is listed after task A but description shows B requires A's output, with no explicit "depends on A" annotation | Hidden dependency |
| Plan ordering shows feature task before its enabling refactor, with no rationale for the inversion | Sequencing problem |
| Foundation task (schema migration, dependency upgrade) scheduled after feature tasks that consume it | Sequencing problem |
| Plan structured as "Task 1: build entire DB schema; Task 2: build all API endpoints; Task 3: build all UI" — each task spans the full breadth of one layer with no end-to-end deliverable until the last task | Vertical-slice absence |
| Multi-feature plan groups all backend work as one phase, all frontend work as another phase, with no individual user-facing flow completable until both phases finish | Vertical-slice absence |
| Single task bundles "refactor module + add new feature + update documentation" as one work item | Scope bundling |
| Tasks list shows estimates of 1h, 8h, 20h, 2h with no explanation for the variance across similar-sized work | Effort uncalibrated |
| Plan has no effort indicator on any task | Effort uncalibrated |
| A task is listed with no link to a parent goal, spec section, or owner | Orphaned task |
| Plan schedules "add caching layer", "generalize API for future use cases", or "extract framework" before the MVP behavior is verified end-to-end | Premature optimization in plan |
| Plan covers complex multi-system migration with no risks/mitigations table, no open-questions section, no enumerated unknowns | Risk register silence |
| Plan depends on an external team / vendor / spec finalization with no statement of the dependency, no fallback if it slips | Risk register silence |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
