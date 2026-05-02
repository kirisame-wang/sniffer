# Bad Smell Samples — architecture-review

Catalog of noteworthy patterns this skill recognizes for architecture-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a module path, import statement, layer name, or directory). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual code under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Domain / use-case module directly imports a concrete IO class (`PostgresClient`, `S3Client`, framework HTTP client) instead of depending on an abstract port | Dependency direction violation |
| Service-layer policy class instantiates a concrete framework adapter rather than receiving it through dependency injection or a port interface | Dependency direction violation |
| UI / view component imports the database driver or ORM model directly, bypassing controller and service layers | Layer skip |
| Background job code accesses raw HTTP request / response objects from the web framework, with no service layer between | Layer skip |
| Module A imports module B which imports module A — direct or indirect cycle visible in the import graph | Cyclic module dependency |
| Three-module triangle (A → B → C → A) where breaking any one edge requires nontrivial restructuring | Cyclic module dependency |
| Repository interface in a domain module returns ORM-specific row objects, query builders, or session handles rather than domain entities | Abstraction leak |
| Public function signature exposes filesystem path strings, database connection objects, or transport-protocol types where domain concepts would suffice | Abstraction leak |
| Module named `utils/` or `common/` references domain entities (`User`, `Order`, `Payment`) that should not be its concern | Boundary erosion |
| Domain entity carries serialization annotations (`@JsonProperty`), HTTP response shapes, or database column metadata | Boundary erosion |
| Code outside a module's public API invokes private / protected / `_underscore` / package-private members directly or via reflection (`setAccessible(true)`, `getattr(obj, '_Cls__field')`) — common callers include external modules and test code asserting intermediate state | Boundary erosion |
| Single module encompasses authentication, caching, logging, and unrelated business logic — its name cannot be stated without "and" | God module |
| Module's public surface exposes 30+ unrelated functions covering disparate concerns; no coherent single responsibility | God module |
| Code consumes an external SDK's data classes (third-party API DTOs) directly throughout internal domain code, with no translation layer | Anti-corruption layer absence |
| Database-vendor-specific types (Mongo `BSONDocument`, Postgres `JSONB`) propagate into business logic untouched | Anti-corruption layer absence |
| Folder structure presents `domain/`, `application/`, `infrastructure/` directories, but `domain/` files import from `infrastructure/` freely | Implicit boundary |
| Package layout suggests modular separation, but every package imports every other package transitively — the apparent modularity is decorative | Implicit boundary |
| Frequently-changing feature module is imported by a stable utility / framework / shared-kernel module, forcing the stable module to release on every feature change | Stable dependencies violation |
| Volatile experimental module is depended on by core platform modules; experiments cannot be removed or reshaped without core releases | Stable dependencies violation |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
