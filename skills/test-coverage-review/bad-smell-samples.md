# Bad Smell Samples — test-coverage-review

Catalog of noteworthy patterns this skill recognizes for test-coverage-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a test file path, a test name, a describe/context block, a line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual test artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Function under test has explicit `try/except` for `ValueError` and `IOError`; tests only call the success path | Error path coverage gap |
| Endpoint specs include 4xx and 5xx response shapes; test suite only exercises 200 responses | Error path coverage gap |
| Function takes a list parameter; tests use `[1, 2, 3]` only — no empty list, no null, no extreme size | Happy-path-only coverage |
| Authentication test verifies valid login but not invalid password, locked account, or expired token | Happy-path-only coverage |
| Function explicitly checks `if x < 0` and `if x > MAX`; neither boundary is tested | Boundary case absence |
| Pagination function has off-by-one risk on page boundaries; tests cover middle pages only | Boundary case absence |
| Test calls function and asserts only `assert result is not None` or `expect(fn).not.toThrow()` | Weak assertion |
| Test compares JSON response to an empty `{}` with `assertContains`, missing field-level checks | Weak assertion |
| Test's only assertion is `toMatchSnapshot()` / golden-file equality; the baseline was captured from the code's own output, never written from the spec | Self-referential oracle |
| Test names like `test_1`, `test_case_a`, `it should work`, `it does the thing` | Test name vagueness |
| Describe block named "edge cases" with no per-test name distinguishing which edge | Test name vagueness |
| Tests share a module-level mutable list; running test_b alone fails because test_a populates it | Setup coupling |
| Test uses `beforeAll` to seed state; later tests mutate it without reset | Setup coupling |
| Test_b reads a record created by test_a; running tests in alphabetical order is required | Test interdependence |
| Integration test relies on data left over from a previous test file's run | Test interdependence |
| Unit test for `OrderService.calculate_total` mocks `OrderService.calculate_total` itself or replaces all its collaborators with mocks that return the expected total | Mock over-reliance |
| Test mocks the database, the cache, the external API, and the clock — final assertion only verifies the mock was called | Mock over-reliance |
| Bug ticket fix landed; commit changed production code only, no new test that would have caught the regression | Regression coverage gap |
| `xfail` / `skip` annotation with comment "flaky, fix later" and no issue link or expiry | Flaky test tolerance |
| Test marked with `retry(3)` to mask intermittent failure; no root-cause investigation reference | Flaky test tolerance |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
