# Bad Smell Samples — diagnosability-review

Catalog of noteworthy patterns this skill recognizes for diagnosability-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a log statement, an error class, a runbook section, a metric name). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual diagnosability artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Spec defines a log line as `log.error("operation failed")` with no operation name, identifier, or input context | Log message vagueness |
| Error template: `"unexpected error: {e}"` — relies on stringified exception, no structured fields | Log message vagueness |
| Spec catches `Exception` and re-raises `RuntimeError("processing failed")`, dropping cause and stack | Error context loss |
| Try/except in spec writes `log.warning("error")` and continues with no record of original exception | Error context loss |
| Multi-service request flow described with no request ID, trace ID, or correlation field threading through logs | Correlation gap |
| Async job pipeline emits per-stage logs with no causal ID linking them to the originating request | Correlation gap |
| Log spec says "log full request body for debugging" with no redaction rule for tokens, passwords, or PII | Sensitive data exposure risk |
| Error log includes `Authorization` header value or session cookie verbatim | Sensitive data exposure risk |
| 5xx response handler logs at `INFO`; routine cache hit logs at `ERROR` | Log level miscalibration |
| Spec uses `WARN` for both "validation failed" (expected) and "database unreachable" (severe) without distinction | Log level miscalibration |
| Error response to caller is `{"error": "Something went wrong"}` with no error code, support ID, or category | User-facing error opacity |
| Spec returns generic 500 for all server errors, no distinction between transient and permanent failures | User-facing error opacity |
| Postmortem describes failure but does not capture inputs, version, time window, or environment that triggered it | Reproduction context gap |
| Error report template has fields for "what happened" and "fix" but no "how to reproduce" or "system state" | Reproduction context gap |
| Critical user flow has no documented metric, dashboard panel, or saved query | Metric and dashboard absence |
| SLO defined as "99.9% availability" but no metric named, no dashboard linked | Metric and dashboard absence |
| Spec defines an error class but no alert rule for spike, threshold, or rate-of-change | Alerting silence |
| Latency budget stated in design doc, no alert on regression beyond budget | Alerting silence |
| Known error class listed in catalog with no diagnostic procedure, escalation path, or remediation | Runbook gap |
| Runbook section exists but has no "how to verify the fix" step or success criterion — on-call engineer cannot confirm the incident is resolved | Runbook gap |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
