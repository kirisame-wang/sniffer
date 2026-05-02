# Bad Smell Samples — pipeline-review

Catalog of noteworthy patterns this skill recognizes for pipeline-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a workflow file, job name, step, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual pipeline under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Pipeline builds and deploys but defines no test stage | Stage coverage gap |
| Workflow runs lint and unit tests but skips integration tests, contract tests, or security scan that the project requires | Stage coverage gap |
| Test step's failure is suppressed with `continue-on-error: true` while a green check is reported to PR | Quality gate absence |
| Lint and typecheck run advisory; merge is allowed regardless of result | Quality gate absence |
| Workflow installs dependencies on every run; no dependency cache, no lockfile-keyed restore | Caching efficiency silence |
| Pipeline rebuilds Docker image from scratch with no layer cache, no BuildKit cache mount | Caching efficiency silence |
| Secret passed as plain command-line argument and echoed in run log | Secret handling risk |
| Workflow exposes `secrets.PROD_DEPLOY_KEY` to a job triggered by `pull_request` from forks | Secret handling risk |
| Workflow has no `on:` trigger spec, or triggers on every event without filtering branches/paths | Trigger condition unclear |
| Two workflows define overlapping triggers with no documented relationship | Trigger condition unclear |
| Pipeline failure produces no notification, no PR comment, no Slack/email alert, no required check | Failure visibility gap |
| Scheduled pipeline failure is silent; only person who notices is the next user looking at the dashboard | Failure visibility gap |
| Deploy step does `helm upgrade` / `terraform apply` with no idempotency safeguard against double-run | Idempotency silence |
| Re-running the workflow appends to a queue, double-publishes a release, or leaves orphan resources | Idempotency silence |
| Staging pipeline uses Node 18; production pipeline uses Node 20; no parity statement or migration plan | Environment drift |
| Dev workflow runs full test suite; prod workflow skips integration tests for "speed" | Environment drift |
| Deploy job pushes to production with no documented rollback step, previous-version pointer, or abort hook | Rollback path absence |
| Spec describes forward-only migration with no procedure for restoring the prior state | Rollback path absence |
| GitHub Actions workflow uses default `GITHUB_TOKEN` with `permissions: write-all` for a job that only reads | Permission scope overbroad |
| CI runner / service account has admin role on production cloud account for a build-only step | Permission scope overbroad |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
