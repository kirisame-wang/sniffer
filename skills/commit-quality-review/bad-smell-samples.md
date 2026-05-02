# Bad Smell Samples — commit-quality-review

Catalog of noteworthy patterns this skill recognizes for commit-quality-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a commit hash, branch name, or message line). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual history under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Single commit changes auth flow, bumps a dependency, and reformats a CSS file under one message | Commit atomicity violation |
| Single commit modifies twenty files across unrelated subsystems with no uniting purpose | Commit atomicity violation |
| Commit subject is `fix`, `update`, `wip`, `changes`, `final`, or `asdf` | Commit message vagueness |
| Commit message body is empty or restates the subject in different words | Commit message vagueness |
| Project uses Conventional Commits but commit lacks `type:` prefix or scope where convention requires | Convention violation |
| Commit subject exceeds repository's stated subject-line length and lacks the body's wrapping convention | Convention violation |
| Commit message describes "renamed function and updated callers" — diff already shows that, no rationale | Why-vs-what gap |
| Bug-fix commit cites neither the symptom, the reproduction, nor the linked issue | Why-vs-what gap |
| Pair-programmed or AI-assisted commit with no `Co-Authored-By` trailer or attribution comment | Attribution gap |
| Commit attributes a change to one author when the diff shows generated/agent edits without acknowledgment | Attribution gap |
| Branch contains five consecutive commits: `wip`, `wip 2`, `fix lint`, `address review`, `squash me` | Squash candidate |
| Series of `fix typo` / `oops` / `revert previous` commits surrounding one logical change | Squash candidate |
| Branch shared with reviewers shows force-push (non-fast-forward update) mid-review | History rewrite risk |
| Commit message indicates rebase onto already-pushed shared branch ("rebased on main, force-pushed") | History rewrite risk |
| Branch named `temp`, `test123`, `johns-branch`, `fix-stuff` against a convention of `feature/<id>` or `fix/<id>` | Branch naming drift |
| Branch carries personal initials or local-only identifier where the convention is ticket-based | Branch naming drift |
| Merge commit with empty subject or auto-generated `Merge branch 'X' of …` and no resolution context | Conflict resolution opacity |
| File contains `<<<<<<<`, `=======`, `>>>>>>>` markers committed to history | Conflict resolution opacity |
| Diff adds `.env` containing `DATABASE_PASSWORD=...`, an API key, or a private SSH key | Sensitive content commit |
| Diff adds a 200MB binary, generated build artifact, or screenshot of personal data | Sensitive content commit |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
