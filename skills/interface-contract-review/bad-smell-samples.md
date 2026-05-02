# Bad Smell Samples — interface-contract-review

Catalog of noteworthy patterns this skill recognizes for interface-contract-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (an endpoint path, a schema name, a field path, a response code). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual contract under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Endpoint defined without request or response schema | Schema completeness gap |
| Endpoint has a `200` response schema but no request body schema; the accepted payload is undocumented | Schema completeness gap |
| Field marked `string` with no max length, format, or pattern; null behavior undocumented | Boundary condition silence |
| Array field has no minItems / maxItems, no behavior on empty | Boundary condition silence |
| Same concept appears as `userId` in one endpoint and `user_id` in another, or `id` (numeric) and `id` (UUID) for different resources | Naming inconsistency |
| Endpoints define only happy-path responses; auth failures, validation errors, rate-limit responses missing | Error response gaps |
| Contract has no version field, no `deprecated` markers, and required/optional transitions are undefined | Field evolution unsafe |
| Three endpoints use offset-based pagination, two use cursor-based, one uses page-number — no rationale or migration plan | Pagination convention scattered |
| Endpoints lack auth specification; scope requirements stated for some endpoints but not others | Auth requirement unclear |
| `POST /resource` and `DELETE /resource/{id}` lack idempotency keys or retry semantics | Idempotency unspecified |
| Field is typed as `string` but always carries a structured ID (e.g., `"user:123"`); type misses the structure | Type-precision mismatch |
| Field is typed as `object` (open shape) where the contract elsewhere uses a closed schema for the same data | Type-precision mismatch |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
