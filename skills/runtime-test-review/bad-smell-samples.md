# Bad Smell Samples — runtime-test-review

Catalog of noteworthy patterns this skill recognizes for runtime-test-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a step number, a section, a scenario name, a line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual test artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Step says "go to dashboard" without specifying URL, navigation source, or what dashboard means in context | Reproduction steps incomplete |
| Step jumps from "click button" to "verify result" with no description of intermediate dialog or confirmation step | Reproduction steps incomplete |
| Test scenario assumes a logged-in user but does not state which user, role, or auth mechanism | State precondition silence |
| Scenario requires pre-created records (a completed order, a specific user account) but does not specify which fixture to use or how to seed it | State precondition silence |
| Scenario walks through a checkout flow with no statement of what to watch (console errors? network 5xx? DOM error states?) | Observation channel gap |
| Plan claims "verify search works" without naming the success signal (response shape? rendered count? URL parameter?) | Observation channel gap |
| Report states "feature works as expected" with no screenshot, log excerpt, or recorded evidence | Evidence capture gap |
| Failed scenario noted with "saw an error" — no console log, network response, or stack trace captured | Evidence capture gap |
| Plan tests on Chrome only despite supporting Safari, Firefox; no rationale for narrower scope | Variant coverage gap |
| Plan covers desktop viewport only when the product ships responsive mobile UI | Variant coverage gap |
| Plan tests only the success flow; offline mode, server-error, slow-network paths absent though product handles them | Failure-mode probe absence |
| Permission-denied path silently skipped despite the feature gating on roles | Failure-mode probe absence |
| Plan creates seeded data and runs through; no spec for cleanup, signing out, or clearing local storage | Cleanup specification silence |
| Test mutates a shared environment but does not state the restore step | Cleanup specification silence |
| Step says "after the request, click X" without an explicit wait condition for the request to complete | Timing and ordering ambiguity |
| Async-heavy flow described in numeric steps with no statement of what intermediate state is observable | Timing and ordering ambiguity |
| Plan tests a feature gate without specifying whether the feature flag is enabled or disabled for the test run | Environmental assumption silence |
| Scenario implicitly relies on a specific clock value (today's date) without freezing or noting time-sensitivity | Environmental assumption silence |
| Report concludes "passed" without stating the success criteria | Result interpretation silence |
| Inconclusive runs marked "needs investigation" with no definition of what would mark it pass or fail | Result interpretation silence |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
