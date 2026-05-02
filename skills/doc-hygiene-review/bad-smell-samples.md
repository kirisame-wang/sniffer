# Bad Smell Samples — doc-hygiene-review

Catalog of noteworthy patterns this skill recognizes for doc-hygiene-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a doc path, section heading, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual doc set under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| The same fact (version requirement, configuration default, command syntax, supported-platform list) appears in two or more docs with conflicting values | Doc duplication drift |
| README and CONTRIBUTING both describe the dev environment; their Node version requirements differ; reader has no way to tell which is canonical | Doc duplication drift |
| Doc links to `[architecture](./docs/architecture.md)` but file is at `./docs/arch/overview.md` | Cross-link rot |
| Internal anchor `#installation` referenced from another doc; target heading no longer uses that slug | Cross-link rot |
| Doc opens with technical detail without stating whether it serves end users, integrators, operators, or contributors | Audience unclear |
| Section starts with operator-facing instructions and shifts mid-page to integrator-facing API examples with no signpost between audiences | Audience unclear |
| Doc has no last-updated date, no owner, no review cadence; reader cannot gauge whether the content is current or abandoned | Maintenance signal absent |
| Long-form doc has no deprecation note on sections superseded by newer documents; superseded content remains discoverable as if still authoritative | Maintenance signal absent |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
