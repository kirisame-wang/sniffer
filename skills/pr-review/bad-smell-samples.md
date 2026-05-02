# Bad Smell Samples — pr-review

Catalog of noteworthy patterns this skill recognizes for PR-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a PR section, file path, commit reference, review-thread URL, label, or CI-status row). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual PR under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| PR body is empty, a single sentence, or mechanically restates the title (`fix: update X` → body: "Updates X") | PR description vagueness |
| PR body says "Updates the API" / "Improves performance" / "Fixes the issue" with no detail of what or why | PR description vagueness |
| PR body lists the changes (matches the file-change summary) without stating motivation, business context, or trade-off considered | Why-vs-what gap |
| PR body lists what changed but provides no motivation, business context, or trade-off — the reader cannot understand why this change was made from the body alone | Why-vs-what gap |
| PR has no link to the ticket, issue, RFC, design doc, or originating discussion that motivated the change | Linked issue absence |
| PR body references "the bug" / "the customer request" / "the spec" without naming or linking it; the originating context is unreachable | Linked issue absence |
| PR body has no "how this was tested / how to verify" section; reviewer must guess what was verified | Test plan absence |
| Test plan reads "tested manually" / "covered by existing tests" with no specifics about scenarios, environment, or pass criteria | Test plan absence |
| PR title says "Fix login bug" but diff also adds a new feature, refactors an unrelated module, or bumps dependencies | Scope mismatch |
| PR description claims one focused change; diff spans many files across multiple subsystems with no scope statement reconciling them | Scope mismatch |
| PR has no reviewer assigned, or only a self-assignment when project policy requires peer review | Reviewer assignment gap |
| Assigned reviewer owns no portion of the touched files; CODEOWNERS for the touched paths is unmatched by any assigned reviewer | Reviewer assignment gap |
| Multiple "wip", "address review", "fix typo" commits remain unsquashed when project policy requires clean history before merge | Pre-merge cleanup risk |
| Review threads marked outstanding remain unresolved; merge proceeds via admin override or required-status-check bypass | Pre-merge cleanup risk |
| PR removes a public API method, changes a return type, or breaks a wire format with no breaking-change label, no version-bump note, no migration callout | Breaking change unmarked |
| Database schema migration in the PR with no downstream coordination note, no rollout sequencing in the body | Breaking change unmarked |
| PR makes UI changes (component rewrite, layout shift, copy change) with no screenshot, recording, or visual diff in the body | Evidence absence |
| PR claims performance improvement / regression fix with no benchmark numbers, no before/after measurement, no profiler trace | Evidence absence |
| Project policy requires squash-merge; PR is configured to merge with a merge-commit (or vice versa) without rationale | Merge-strategy / template mismatch |
| PR template checklist has unchecked required boxes (e.g., "Updated docs", "Added tests", "Considered backward compatibility") with no explanation | Merge-strategy / template mismatch |
| PR uses a stale template version that omits sections present in the project's current template | Merge-strategy / template mismatch |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
