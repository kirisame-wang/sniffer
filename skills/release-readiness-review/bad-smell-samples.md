# Bad Smell Samples — release-readiness-review

Catalog of noteworthy patterns this skill recognizes for release-readiness-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a section heading, checklist item, table row, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual plan under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Feature listed in launch scope with no acceptance criterion, no demo gate, no definition of done | Acceptance coverage gap |
| Release plan enumerates ten features; only one has a go-live verification step | Acceptance coverage gap |
| Plan ships a new endpoint with no dashboard panel, no metric, no alert defined ahead of launch | Monitoring prep gap |
| New user-facing feature launches with no error-rate or latency alert in place | Monitoring prep gap |
| Plan describes go-live without a rollback procedure, previous-version pointer, or abort criterion | Rollback prep gap |
| Plan estimates "we'll roll back if needed" without a defined time-to-revert or rollback ownership | Rollback prep gap |
| Plan affects customers with no notification step, no email/in-app/release-notes channel, no lead time | Communication plan absence |
| Internal-only release affects support team workflow with no internal communication step | Communication plan absence |
| New feature launches with no updated runbook, no support script, no FAQ entry | Support readiness gap |
| Plan handoff to on-call mentioned in passing; no rotation owner identified, no training noted | Support readiness gap |
| Feature gated behind a flag; default value, ramp schedule, kill switch, and target audience undefined | Feature flag plan unclear |
| Flag mentioned in plan but no owner, no expiry/cleanup criterion, no monitoring of flag-on vs flag-off cohorts | Feature flag plan unclear |
| Plan launches a high-traffic surface with no statement of expected RPS, payload mix, or headroom check | Capacity assumption silence |
| Capacity check listed as a step with no target value, no current baseline, no escalation criterion | Capacity assumption silence |
| Plan checklist includes "Security review" / "Legal review" with no owner assigned, no completion mark | Sign-off gap |
| Privacy / accessibility review required by policy is missing from the checklist entirely | Sign-off gap |
| Plan ends at "deploy"; no smoke-test list, no metric thresholds, no post-launch verification window | Post-launch verification gap |
| Post-launch step says "monitor for issues" with no definition of issue, no thresholds, no escalation criterion | Post-launch verification gap |
| Plan does not name the go / no-go decision-maker, the launch driver, or the on-call owner during the launch window | Ownership unclear |
| Multiple owners listed for the same step with no primary; escalation path during launch undefined | Ownership unclear |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
