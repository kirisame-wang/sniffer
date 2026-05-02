---
name: security-review
description: Sniffs bad smells in security artifacts and source code (threat models, auth/authz specs, hardening guides, source modules with security-sensitive logic) without exploiting them. Reports the few concentrations that matter; holds full evidence in working memory. Use when a security spec is finalized, when security-sensitive code is up for review, or when reviewing a contributor's security artifact.
phase: Review
---

# security-review

## Overview

A graybox audit skill for security-relevant artifacts. Only the few concentrations that warrant the reviewer's next move surface in the artifact; the complete observation set lives in working memory and emerges on request.

This skill audits the structural quality of a security posture as documented or coded: threat coverage, authentication, authorization, input handling, secret management, encryption, auditability. The substance of whether the threat model captures the right risks (threat-modeling judgment), whether an exploit actually works (penetration testing), and whether the runtime stack is patched (vulnerability scanning) belong to other audit domains.

## When to Use

- A threat model, auth/authz specification, or hardening guide is finalized
- Security-sensitive source code (auth, crypto, input handling, session management) is up for review
- A contributor's security artifact is up for review before merge

**When NOT to use:** active penetration testing or exploit development (operational red-team work, not a graybox audit); evaluating runtime CVE exposure (that's a vulnerability scanner's job); auditing performance characteristics of crypto code (that's `performance-review`'s domain).

## Smell Profile

Named categories used as report vocabulary:

- **Threat coverage gap** — threat model omits a major category (spoofing, tampering, repudiation, info-disclosure, DoS, elevation) or a known surface (auth, ingress, persistence)
- **Authentication weakness** — password policy unstated, MFA optional without rationale, session lifetime / rotation undefined, credential stuffing unconsidered
- **Authorization gap** — endpoint or function lacks role/scope check, or scope check lives in client/UI only
- **Input validation silence** — untrusted input enters without length, type, format, encoding, or sanitization spec
- **Output encoding gap** — user-controlled data flows into HTML, SQL, shell, template, or downstream sink without context-appropriate encoding, escaping, or auto-sanitization spec
- **Secret management gap** — credentials, API keys, tokens stored in code, env, config, or logs without rotation, vault, or scoping plan
- **Encryption gap** — TLS/at-rest/in-transit encryption unspecified, key management undocumented, weak primitives or algorithms named
- **Audit log gap** — security-relevant events (login, logout, privilege change, sensitive read/write) are not logged or are logged without identity/time
- **Trust boundary unclear** — what crosses internal/external/admin/user boundaries is unspecified; the perimeter is undefined
- **Supply-chain silence** — third-party dependency choices, version pinning, license review, CVE monitoring undocumented
- **Incident response gap** — handling for security incident (compromise, data leak, abuse) has no documented procedure, escalation, or notification path

Detailed patterns and recognition examples for each category live in [`bad-smell-samples.md`](bad-smell-samples.md). The Smell Profile gives vocabulary; the catalog gives recognition shapes — clustering of multiple categories on shared evidence is a runtime judgment from the actual artifact under review, not a pre-authored combination. Each observation carries a category tag and an evidence anchor (a file path, function, section, or line range). Anything that fits none of the named categories is held under "miscellaneous".

## Input Contract

A security artifact (threat model, security review document, auth/authz specification, hardening guide) or security-sensitive source code (auth flow, crypto, input handler, session manager). Scope-ambiguous cases require reviewer confirmation rather than silent picking. The audit covers the artifact(s) only — no live exploit attempts, no runtime traffic, no external links — with cross-reference limited to in-scope documents.

## Output Format

Pre-delivery, the artifact aligns with Smell Profile, Input Contract, and Out of Scope. Report-file appendage is reviewer-triggered only.

```markdown
# Security Review — <artifact name>

**Date:** <YYYY-MM-DD>
**Documents in scope:** <paths>
**Reviewer:** sniffer/security-review

## Top concentrations

### 1. <short noun-phrase title>

<One-line factual observation, with evidence inline as file path, section, or line range.>

Smells converging: <category A>, <category B>, …

> **→ Investigate:** <a non-directive question for the reviewer>

### 2. <…>

(Cluster count tracks evidence — typically 1-3, more if warranted. No padding to a target, no capping below evidence.)

## Coverage

All named categories were scanned. Observations beyond the top concentrations are held in working memory; any category, location, or finding is expandable on request.

*Passed lenses (no findings): <list — present only when at least one category has zero observations>.*
```

## Out of Scope (during the audit / in the artifact)

The written report does **not** include:

- Active exploit attempts, proof-of-concept payloads, or pen-test execution
- CVE database lookups or runtime vulnerability scans
- Recommendations to add, remove, or restructure security controls beyond surfacing the smell
- Performance evaluation of crypto or auth implementations
- Content loaded from external links or third-party docs

These boundaries apply to **report content**. After delivery, follow-up conversation is normal interaction — opinions, fixes, or guidance are available on reviewer request.

## References

- [bad-smell-samples.md](bad-smell-samples.md) — recognition catalog (one pattern per category); resource for category-uncertain observations
