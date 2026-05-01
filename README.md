# sniffer

> Smell for me.

AI agent skills that sniff bad smells across your software development lifecycle — without opening the box.

Each skill is a trained nose for one specific audit domain: architecture drift, interface gaps, test blind spots, behavior mismatches. The agent reports what it detects. **You decide what to open.**

## What is graybox verification?

Graybox sits between blackbox (you see nothing) and whitebox (you see everything). The skill collects just enough structured information for an experienced reviewer to form a judgment — then stops. **No conclusions, no scores, no recommendations.**

Bad smells are yours to define. The sniffer just flags where to look.

**Equivalent terminology**: graybox equals progressive disclosure applied to audit outputs.

## Skills

Skills are flat under `skills/` (no phase subdirectories). Below is the navigation by SDLC phase, following [agentskills.io](https://agentskills.io/home) open-standard conventions.

| Phase | Skill | Status | Audit domain |
|------|------|------|------|
| Define | `design-doc-review` ✨**MVP** | planned | design-doc smells (specs / ADRs / RFCs) |
| Define | `skill-design-review` | planned | SKILL.md design quality (sniffer-original) |
| Plan | `task-decomposition-review` | planned | task breakdown quality |
| Build | `interface-contract-review` | planned | contract smells (API / schema / type) |
| Build | `ui-quality-review` | planned | UI quality (accessibility / responsiveness) |
| Verify | `test-coverage-review` | planned | test quality |
| Verify | `runtime-test-review` | planned | runtime test smells |
| Verify | `diagnosability-review` | planned | observability gaps |
| Review | `complexity-review` | planned | maintainability smells |
| Review | `security-review` | planned | security smells |
| Review | `performance-review` | planned | performance smells |
| Ship | `commit-quality-review` | planned | commit / change quality |
| Ship | `pipeline-review` | planned | CI/CD soundness |
| Ship | `migration-risk-review` | planned | deprecation / migration risk |
| Ship | `doc-alignment-review` | planned | doc-vs-implementation drift |
| Ship | `release-readiness-review` | planned | release readiness |

## Quick start

A sniffer skill is a folder with `SKILL.md` (Anthropic Agent Skills format). Once installed in your agent (Claude Code, Cursor, Goose, etc. — see [client showcase](https://agentskills.io/home)), invoke a skill against an artifact:

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

- [Anthropic Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) — authoritative format spec
- [agentskills.io](https://agentskills.io/home) — open cross-vendor standard
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — reference implementation; sniffer's skill list mirrors its skill domains for SDLC coverage
