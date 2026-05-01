# Bad Smell Samples — skill-design-review

Catalog of noteworthy patterns this skill recognizes for SKILL.md-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a frontmatter field, a section heading, a paragraph, a recurring phrase). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual SKILL.md under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| `name: claude-helper` — uses Anthropic-reserved word | Frontmatter convention violation |
| `description: Helps with files` — vague, no what + when | Frontmatter convention violation |
| `description: I can help you process documents` — first-person POV; description is injected into system prompt where POV mismatch breaks discovery | Frontmatter convention violation |
| Skill description lists 6 different audit domains it handles | Frontmatter convention violation (single responsibility) |
| SKILL.md has Process and Examples but no `Out of Scope` and no `Output Format` | Missing boundary anchor |
| SKILL.md uses numbered sequential labels (e.g., `Step 1 / 2 / 3`, `Phase 1 / 2 / 3`) to enumerate workflows the agent can self-derive from the responsibility statement | HOW prescription (sequence enumeration) |
| SKILL.md text uses pervasive `ALWAYS` / `NEVER` / `MUST` in all-caps; commands replace explanation | HOW prescription (surface signal) |
| SKILL.md says "first run sibling-skill-X to generate input, then this skill consumes it" | Cross-skill hard dependency |
| DoD requires running a skill that is not in the project's skill list | Cross-skill hard dependency (absent target) |
| SKILL.md fails one of: interface clarity / internal encapsulation / single responsibility / independent execution / low coupling | Five-criteria self-validation failure |
| Smell Profile lists 9+ named categories spanning frontmatter conventions, output format, doc length, and boundary semantics — heterogeneous concerns under one skill | Five-criteria self-validation failure (single responsibility, surface signal) |
| Out of Scope says "no recommendations"; agent applies it to follow-up chat as well, refusing to opine when asked | Boundary scope leak |
| Coverage section dumps `completeness: 3 · contradictions: 4 · ...` raw counts; reader cannot tell which counts matter | Boundary scope leak (artifact-form rule violated) |
| SKILL.md is 430 lines with full Examples (90 lines), Common Rationalizations (40 rows), and Bad Smell Samples (30 rows) all inline | Length sprawl |
| Reference file `api-reference.md` is 220 lines without a Table of Contents at top | Length sprawl (missing ToC) |
| SKILL.md links to `advanced.md`, which links to `details.md`, which contains the actual content (3-layer nesting) | Length sprawl (nested references) |
| SKILL.md uses `@<path>` prefix in References instead of standard markdown links | Reference format violation |
| File path written as `scripts\helper.py` (Windows backslash) | Reference format violation |
| SKILL.md or a skill-local file contains markdown links to `../../docs/...`, `../sibling-skill/...`, or absolute paths — links 404 when the skill is installed in a project lacking the same folder structure | Reference format violation (out-of-folder link) |
| `bad-smell-samples.md` row tells a project story (specific incident, named artifact, dated change) instead of stating a generic recognition shape | Sample memoir |
| A smell category occupies 3 rows that paraphrase one core shape with different surface examples — each row teaches the same recognition under a different guise | Sample redundancy |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
