# Bad Smell Samples — security-review

Catalog of noteworthy patterns this skill recognizes for security-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a file path, function, section, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Threat model lists "spoofing" and "tampering" but omits "repudiation" and "elevation of privilege" with no rationale | Threat coverage gap |
| Threat model covers external attackers but not insider threats, compromised dependencies, or admin-account abuse | Threat coverage gap |
| Auth spec omits password length / complexity / breach-list check; MFA listed as "optional" with no rationale | Authentication weakness |
| Session lifetime, refresh-token rotation, and revocation flow undefined | Authentication weakness |
| Endpoint spec defines functionality but no scope/role check; role gating is described as "the UI hides the button" | Authorization gap |
| Admin endpoint enforces role at controller layer; service layer accepts any caller and is reused by background job | Authorization gap |
| Input field accepts free-form string with no max length, character set, encoding, or canonicalization rule | Input validation silence |
| Boundary between trusted and untrusted input is undefined; spec treats all upstream sources as pre-validated | Input validation silence |
| User-supplied content rendered in HTML / SQL / shell / template without explicit context-aware encoding spec | Output encoding gap |
| Spec uses `innerHTML`, raw template interpolation, or `dangerouslySetInnerHTML` with user data and no sanitization library named | Output encoding gap |
| API key or password literal embedded in source file or example config committed to the repository | Secret management gap |
| Spec says "store credentials in env" with no rotation cadence, vault, or scoping plan | Secret management gap |
| Spec describes data flow without naming TLS for in-transit or encryption-at-rest for the data store | Encryption gap |
| Algorithm specified as MD5 / SHA1 / DES / ECB-mode / hand-rolled crypto for security-sensitive use | Encryption gap |
| Privilege change, sudo, password reset, or sensitive-record read happens with no audit log entry | Audit log gap |
| Log entries for security-relevant events lack actor identity, timestamp, source IP, or correlation ID | Audit log gap |
| System diagram shows multiple components but does not mark which boundary is internal vs external vs admin | Trust boundary unclear |
| Internal service trusts request headers (`X-User-Id`) populated by the gateway with no signing or verification | Trust boundary unclear |
| Dependency manifest pinned to a major version range with no policy on CVE monitoring or upgrade cadence | Supply-chain silence |
| Spec uses third-party SDK with no review of its permission model, network calls, or license | Supply-chain silence |
| Hardening guide ends at "follow best practices"; no procedure for credential compromise, breach notification, or revocation | Incident response gap |
| Audit logs are emitted but the document defines no procedure for monitoring, alerting, or post-incident retention | Incident response gap |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
