# Bad Smell Samples — performance-review

Catalog of noteworthy patterns this skill recognizes for performance-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a file path, function, section, or line range). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Spec describes a critical request path with no latency target, throughput target, or memory ceiling | Budget absence |
| Hot loop in code path with no documented per-call budget despite production traffic | Budget absence |
| Benchmark report claims "12% faster" with no description of workload, hardware, warmup, or run count | Measurement methodology silence |
| Profile flame-graph attached with no statement of which workload or scenario produced it | Measurement methodology silence |
| Capacity plan estimates instance count but does not state assumed RPS, payload size, or burst factor | Workload assumption silence |
| Cache sizing computed without naming the access distribution (uniform, Zipf, hot-set fraction) | Workload assumption silence |
| Nested loop scans collection-of-collections; outer and inner sizes both grow with input | Algorithmic complexity smell |
| Sort-then-search-linearly pattern, or repeated `O(n)` membership check inside a loop | Algorithmic complexity smell |
| Loop over user list issues one DB query per user (`for u in users: db.fetch(u.profile_id)`) | Chatty access pattern |
| Map-render loop issues one downstream RPC per item with no batching or prefetch | Chatty access pattern |
| Cache layer added with no TTL, no invalidation rule, no behavior on miss, no stampede control | Caching specification gap |
| Spec mentions caching but does not define cache key, key-space size, or eviction policy | Caching specification gap |
| Database connection opened in a loop with no `close` / `with` / pool; risk of exhaustion under load | Resource lifecycle risk |
| Goroutine / thread / async task spawned per request with no bound, no cancellation, no shutdown | Resource lifecycle risk |
| Concurrent module with shared mutable state, no lock ordering documented, multiple lock acquisition paths | Concurrency design unclear |
| Producer-consumer flow with no backpressure, no buffer bound, no behavior when consumer falls behind | Concurrency design unclear |
| Hand-rolled lock-free structure introduced for "perf" with no profile showing the lock was contended | Premature optimization |
| Custom allocator, struct-packing macro, or SIMD intrinsic added with no benchmark baseline | Premature optimization |
| SLO defined for the path but no CI gate, no production alert, no baseline metric tracked | Regression guard silence |
| Benchmark suite exists but is run ad-hoc, with no history, no comparison-against-baseline policy | Regression guard silence |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
