# sniffer

*繁體中文版本請見 [README-zh.md](README-zh.md)*

> Smell for me.

AI agent skills that sniff bad smells across your software development lifecycle — without opening the box.

Each skill is a trained nose for one specific audit domain: architecture drift, interface gaps, test blind spots, behavior mismatches. The agent reports what it detects. **You decide what to open.**

## What is graybox verification?

Graybox sits between blackbox (you see nothing) and whitebox (you see everything). Too much information drowns the reviewer in detail; too little hides every smell. The skill collects just enough for an experienced reviewer to form a judgment — then stops. **No conclusions, no scores, no recommendations.**

**The reviewer is the actor, not the agent.** Bad smells are yours to define; the sniffer flags where to look. A report succeeds when the reviewer knows what to ask next — or knows it's safe to ship; not when it enumerates every possible problem. Three rules express this division of labor:

- **Summary, not verdict** — output is observed facts. The reviewer identifies the smell.
- **Locate, not expand** — point to where, not what. The reviewer decides whether to dig.
- **Consistent disclosure depth** — each skill stays at its layer. An X-ray reads bones, not a full-body scan.

All three rules pin the skill to a stopping point.

### Why stop?

Stopping is a deliberate design choice, not a compromise on completeness:

- Whitebox review isn't a more complete graybox review — it's a different activity.
- When information is fully transparent, strategic judgment dissolves into line-by-line comparison.
- Investigation is a deliberate act. The report identifies where the smell concentrates; pursuing it remains the reviewer's judgment.

### How it works

The audit is diagnostic, not exhaustive. Each skill carries a trained catalog of smells — patterns specific to its audit domain — and reports where the target matches them. Some skills also detect drift through *artifact pairing* (spec vs code, design vs plan), where divergence between two related artifacts is itself a smell. Partial information suffices because the goal is to narrow uncertainty, not certify completeness.

So sniffer isn't a CI gate, continuous monitoring, or a line-by-line review tool — it's a pre-review nose, invoked when a stage artifact lands, to help the reviewer decide where to spend attention.

The same shape — load minimum signal, expand on demand — repeats at three layers. Anthropic uses progressive disclosure for skill *loading*; sniffer extends it to skill *output* and to *documentation*:

| Layer | Always loaded | Loaded on trigger | Loaded on demand |
|------|--------------|-------------------|------------------|
| Skill loading | name + description metadata | SKILL.md body | bundled scripts / references |
| Skill output | top concentrations + coverage | full observations in working memory | reviewer asks → expanded |
| Documentation | main text (decisions + scope) | `references/<topic>.md` | follow link to read |

## Skills

Skills are flat under `skills/` (no phase subdirectories). Below is the navigation by SDLC phase, following [agentskills.io](https://agentskills.io/home) open-standard conventions.

| Phase | Skill | Status | Audit domain |
|------|------|------|------|
| Define | [`requirements-doc-review`](skills/requirements-doc-review/SKILL.md) | v1 | requirements clarity & verifiability (problem space) |
| Define | [`design-doc-review`](skills/design-doc-review/SKILL.md) | v1 | design-doc soundness; + requirements-traceability when in scope |
| Plan | [`task-decomposition-review`](skills/task-decomposition-review/SKILL.md) | v1 | task-list structural quality (atomicity / sequencing / vertical slicing) |
| Plan | [`implementation-plan-review`](skills/implementation-plan-review/SKILL.md) | v1 | plan structural readiness for design-conformance; + design-vs-plan inheritance when in scope |
| Build | [`interface-contract-review`](skills/interface-contract-review/SKILL.md) | v1 | contract smells (API / schema / type) |
| Build | [`ui-quality-review`](skills/ui-quality-review/SKILL.md) | v1 | UI quality (accessibility / responsiveness / state coverage) |
| Verify | [`test-coverage-review`](skills/test-coverage-review/SKILL.md) | v1 | test-suite quality |
| Verify | [`runtime-test-review`](skills/runtime-test-review/SKILL.md) | v1 | runtime test plans / sessions |
| Verify | [`diagnosability-review`](skills/diagnosability-review/SKILL.md) | v1 | observability / runbook gaps |
| Review | [`complexity-review`](skills/complexity-review/SKILL.md) | v1 | structural maintainability |
| Review | [`architecture-review`](skills/architecture-review/SKILL.md) | v1 | module boundaries / dependency direction (sniffer-original) |
| Review | [`security-review`](skills/security-review/SKILL.md) | v1 | security posture |
| Review | [`performance-review`](skills/performance-review/SKILL.md) | v1 | performance posture |
| Review | [`doc-alignment-review`](skills/doc-alignment-review/SKILL.md) | v1 | doc-vs-spec/code alignment |
| Review | [`doc-hygiene-review`](skills/doc-hygiene-review/SKILL.md) | v1 | doc-set hygiene (consistency / links / audience / lifecycle) |
| Review | [`skill-design-review`](skills/skill-design-review/SKILL.md) | v1 | SKILL.md design quality (sniffer-original meta) |
| Ship | [`commit-quality-review`](skills/commit-quality-review/SKILL.md) | v1 | commit-history quality |
| Ship | [`pr-review`](skills/pr-review/SKILL.md) | v1 | PR merge-readiness (sniffer-original) |
| Ship | [`pipeline-review`](skills/pipeline-review/SKILL.md) | v1 | CI/CD soundness |
| Ship | [`migration-risk-review`](skills/migration-risk-review/SKILL.md) | v1 | deprecation / migration risk |
| Ship | [`release-readiness-review`](skills/release-readiness-review/SKILL.md) | v1 | release readiness |

## Quick Start

<details>
<summary><b>Claude Code (recommended)</b></summary>

**Marketplace install:**

```
/plugin marketplace add kirisame-wang/sniffer
/plugin install sniffer@sniffer-marketplace
```

> **SSH errors?** The marketplace clones repos via SSH. If you don't have SSH keys set up on GitHub, either [add your SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) or switch to HTTPS for fetches only:
> ```bash
> git config --global url."https://github.com/".insteadOf "git@github.com:"
> ```

</details>

A sniffer skill is a folder with `SKILL.md` (Anthropic Agent Skills format). Once installed, invoke a skill against an artifact:

```
> Use design-doc-review on docs/architecture.md
```

The agent emits a compact `*-review.md` report with **top concentrations** (multi-category smell clusters) and a **coverage** summary. Full observation set stays in working memory — ask follow-ups (`expand contradictions`, `what's near §3?`, `how would you fix #2?`) and the agent surfaces matching observations in chat.

The audit phase ends with the report. **Follow-up conversation is normal interaction** — the agent can give opinions, suggestions, and prioritization when asked.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md). Two contribution paths:

- **Submit a bad-smell sample** — append to an existing skill's `bad-smell-samples.md`
- **Submit a new skill** — follow the SKILL.md template; must pass `skill-design-review` self-audit before merge

## Inspirations

- **Matt Pocock — *Design the interface, delegate the implementation* (AI Engineer, 2026-04).** sniffer extends his "gray boxes" idea — modules trusted by interface and tests, not line-by-line — from modules to audit outputs.
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — reference implementation. sniffer mirrors its skill domains where an audit twin is meaningful (their skills *produce*, sniffer *audits the production*)
- [Anthropic Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) — authoritative format spec
- [agentskills.io](https://agentskills.io/home) — open cross-vendor standard
