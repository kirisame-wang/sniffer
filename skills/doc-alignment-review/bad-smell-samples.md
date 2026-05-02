# Bad Smell Samples — doc-alignment-review

Catalog of noteworthy patterns this skill recognizes for doc-alignment-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a doc path, section heading, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| README example shows function signature `f(x, y)`; in-scope source defines `f(x, y, z)` | Spec / code drift |
| API reference describes endpoint returning `{success, data}`; OpenAPI spec returns `{ok, payload}` | Spec / code drift |
| Architecture documentation (ADR text or diagram) asserts a layering or boundary policy; the actual import graph contradicts it | Spec / code drift |
| Doc references "v2.3" or "release/2024-01" while the in-scope artifact has moved beyond that version | Stale information |
| Quickstart references a CLI flag that the in-scope tool's `--help` no longer lists | Stale information |
| Quickstart steps 1-5 do not produce a runnable state when traced against the in-scope artifact | Quickstart / install gap |
| Install guide references a setup script that the repository does not contain | Quickstart / install gap |
| Public function in source has no doc-comment and no entry in API reference | API doc gap |
| API reference lists methods (`oldMethod`, `legacyHelper`) that no longer exist in source | API doc gap |
| Architecture diagram shows component "AuthService"; prose calls the same component "User Authentication Layer" | Diagram-text mismatch |
| Sequence diagram shows three steps; prose describes four steps in different order | Diagram-text mismatch |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
