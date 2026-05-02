# Bad Smell Samples — migration-risk-review

Catalog of noteworthy patterns this skill recognizes for migration-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a section heading, step number, table row, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual plan under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Plan replaces an API endpoint with no statement of what existing callers receive during or after the cutover | Backward compatibility silence |
| Schema change drops a column; plan does not address services or queries still reading it | Backward compatibility silence |
| Deprecation plan announces sunset date for the old path; replacement is described as "in development" or covers a subset of existing use cases | Replacement readiness gap |
| Migration plan points consumers to a replacement API/library/service that has no production traffic, no parity test against the old path, no feature-coverage statement | Replacement readiness gap |
| Plan describes forward migration only — no statement of how to revert, no criterion for triggering rollback | Rollback path absence |
| Schema change is destructive (drop column / rename table); plan has no recovery procedure | Rollback path absence |
| Backfill step listed without a reconciliation check (count match, checksum, sampled diff) | Data integrity gap |
| Dual-write phase mentioned with no spec for handling write conflicts or read consistency | Data integrity gap |
| Plan flips all traffic from old to new in one step with no canary, percentage rollout, or feature-flag gating | Phased rollout absence |
| Multi-region deploy is "all regions, simultaneous"; no per-region staging or cohort | Phased rollout absence |
| API deprecated with no sunset date, no end-of-support announcement, no hard cutoff for old callers | Deprecation timeline silence |
| Plan says "old field will be removed eventually" with no timeline or criterion for removal | Deprecation timeline silence |
| Plan affects external API consumers with no defined notification step, channel, or lead time | Communication plan absence |
| Schema change affects downstream analytics with no notice to data team listed | Communication plan absence |
| Migration runs to completion; plan defines no acceptance check, no validation query, no sign-off step | Verification step gap |
| "Verify in production after rollout" with no spec of what to verify or what counts as success | Verification step gap |
| Old and new versions live simultaneously; spec is silent on which write wins, how reads merge, or how conflicts resolve | Concurrent state risk |
| Dual-read phase with no rule for which source is authoritative when they disagree | Concurrent state risk |
| Migration script applied as `INSERT` / `CREATE` without `IF NOT EXISTS` or transaction guard; re-running may double-apply | Idempotency silence |
| Plan does not state whether the migration script is safe to re-run after partial failure | Idempotency silence |
| Plan involves DB schema change, service deploy, and client release with no order constraints stated | Dependency ordering unclear |
| Plan says "deploy in any order"; new service requires a column the schema migration adds | Dependency ordering unclear |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
