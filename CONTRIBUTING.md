# Contributing to sniffer

Two paths to contribute. Pick the one that matches what you have:

- **You hit a real bad smell** that an existing skill missed → [submit a sample](#submit-a-bad-smell-sample)
- **You want to add a new audit domain** that no skill covers → [submit a new skill](#submit-a-new-skill)

For project background and the skill roadmap, read [`README.md`](README.md) first.

> **Note**: Two reference skills (`design-doc-review`, `skill-design-review`) are now available under `skills/`. Detailed convention docs (skill design rules, SKILL.md template, DoD checklist, authoritative spec references) arrive in subsequent commits. The high-level rules below are stable; specific file paths to those convention docs will be added as they land.

---

## Submit a bad-smell sample

The fastest contribution path. You append a generic recognition pattern to an existing skill's `bad-smell-samples.md`.

1. Identify which skill should have caught the smell (see the skill table in [`README.md`](README.md))
2. Open `skills/<skill-name>/bad-smell-samples.md`
3. Append a row describing the pattern:
   - **Pattern** column: a generic, portable description of what makes this smell recognizable (no project-specific incidents or named artifacts)
   - **Smell category** column: the matching named category from the skill's Smell Profile

Each row should describe one generic shape matching one named category. Avoid project memoirs (specific incidents, dated changes, named artifacts) — patterns must be portable across projects. Avoid near-duplicates of existing rows — if your pattern shares the same underlying shape as an existing row, the catalog already covers it. Whether multiple smells cluster on shared evidence to form a Layer 1 concentration is a runtime judgment, not pre-authored here.

4. Open a PR with the row + a short note on where you saw an instance (the note explains motivation; the row itself stays generic)

---

## Submit a new skill

A heavier contribution. New skills must follow project conventions and pass self-audit before merge.

### Steps

1. **Confirm the audit domain doesn't overlap** with an existing or planned skill (see the skill table in [`README.md`](README.md)). If smells your skill would catch are already covered, propose extending the existing skill instead.

2. **Create the skill folder**: `skills/<your-skill-name>/SKILL.md`. Naming: lowercase + hyphens, gerund or noun-phrase form, no reserved words (`anthropic`, `claude`).

3. **Write the SKILL.md** following the template (template doc lands in subsequent commits). Required sections at minimum: Frontmatter (`name`, `description`, `phase`), Overview, When to Use, Responsibilities, Smell Profile, Input Contract, Output Format, Out of Scope, Common Rationalizations, Red Flags, Verification.

4. **Pass DoD self-audit** — three hard gates, all required:
   - **Gate 1** — run `skill-design-review` on your `SKILL.md` and attach the sniff report
   - **Gate 2** — pass the five self-validation criteria: interface clarity / internal encapsulation / single responsibility / independent execution / low coupling
   - **Gate 3** — invoke the skill on a real (or minimal synthetic) artifact and confirm the output matches the `## Output Format` spec, shows no `## Red Flags` symptoms, and passes the `## Verification` checklist

5. **Populate `bad-smell-samples.md`** with at least one canonical row per named category in your Smell Profile. Generic patterns; no project memoirs; no near-duplicates.

6. **Open a PR** with: `SKILL.md`, `bad-smell-samples.md`, dogfood report, and a short rationale (which agent-skills source you mirrored, if any; or why this is a sniffer-original).

### What gets rejected

Skills that fail DoD on:

- **Scope sprawl** — covers multiple audit domains in one skill (split into siblings)
- **HOW prescription** — teaches the agent step-by-step methodology instead of declaring responsibilities; surface signal: pervasive `ALWAYS`/`NEVER`/`MUST` in all-caps
- **Cross-skill hard dependency** — requires another sibling skill at runtime
- **Boundary leak** — Out of Scope is missing, vague, or extends into follow-up chat
- **Convention violations** — frontmatter format, naming, reference paths, line limits

---

## Conventions

- **Markdown links** — use standard `[label](path)` form for navigation, not `@<path>` prefix
- **Forward slashes** — always; no Windows-style backslashes
- **Description field** — third person, includes both *what* and *when*; specific enough to win in competitive 100+-skill triggering (generic phrasing undertriggers even when scope is correct)
- **References one level deep** — link from SKILL.md directly to supporting files; avoid nested chains
- **Reference files >100 lines** — include a Table of Contents at top
- **SKILL.md soft target 100 lines / hard warning 250 lines** — sniffer is stricter than Anthropic's 500-line guideline due to the graybox attention budget

Authoritative cross-vendor conventions: [Anthropic Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) + [agentskills.io open standard](https://agentskills.io/home).

---

## Questions?

For project-design questions, open an issue tagged `design`. For skill-authoring questions, tag `skill-author`. For bad-smell calibration debates, tag `calibration`.
