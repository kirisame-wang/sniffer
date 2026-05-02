# Bad Smell Samples — complexity-review

Catalog of noteworthy patterns this skill recognizes for code-complexity-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a file path, function name, line range, or symbol). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual source code under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Function spans 200+ lines with no internal decomposition; sibling functions in the same file are 20-40 lines | Long function |
| Function bundles parsing, validation, transformation, side effects, and response shaping in one body | Long function |
| Five levels of nested `if` / `for` / `try` inside one function | Deep nesting |
| Loop containing nested conditional containing nested loop containing another conditional | Deep nesting |
| Same retry-with-backoff loop appears verbatim in three different services | Duplicated logic |
| Same null-check + default-fallback chain repeats across many methods of one class | Duplicated logic |
| One class contains user CRUD, payment processing, email sending, and audit logging | Large class / large module |
| Module exports 40+ unrelated functions; cohesion across them is low | Large class / large module |
| Function defined in module, never imported anywhere; no test references it | Dead code |
| `if False:` block, unreachable branch after early return, or `# TODO: remove` block kept for years | Dead code |
| `BaseRepository` abstract class with one concrete implementation, no plan for a second | Premature abstraction |
| Single-call hook indirection: `getHandler()` returns the only handler that exists | Premature abstraction |
| Function takes `**kwargs` and `**options` but no caller passes anything beyond two known keys | Speculative generality |
| Plugin registry with extension points; only one built-in plugin ships, no external one is planned | Speculative generality |
| `def f(a, b, c, d, e, f, g, h)` — eight required positional parameters with no grouping object | Long parameter list |
| Function takes seven booleans flag-style; callers pass mix of literal `True`/`False` | Long parameter list |
| `module_a` imports `module_b`; `module_b` imports `module_a` (direct cycle) | Cyclic dependency |
| Package `core` depends on `utils`; `utils` depends on `core.types` (transitive cycle through subpackage) | Cyclic dependency |
| Function named `process()`, `handle()`, `do_it()` with no domain noun in the name | Naming opacity |
| Variable named `temp`, `data`, `obj`, `x` carrying domain-specific entity through a multi-line block | Naming opacity |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
