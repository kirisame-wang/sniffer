# Bad Smell Samples — design-doc-review

Catalog of noteworthy patterns this skill recognizes, one per named smell category. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors. Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual document under review** — not a pre-authored combination. Clusters emerge from the document's real evidence rather than from matching pre-canned concentrations.

| Pattern | Smell category |
|---|---|
| Doc has "Goals" and "Architecture" but no "Success Criteria" or "Open Questions" | Completeness gap |
| Text states "we use Postgres" with no Context or Alternatives nearby | Undocumented decision |
| "TBD" / "tentative" / "to be confirmed" appears 5+ times, none with owner or deadline | Unresolved decision |
| Two sections share the same ordinal (e.g., duplicate "Section 2"); or a "see Section 4" reference points to obsolete content | Internal contradiction |
| Doc declares "we organize by X" but the directory, ToC, or downstream tables don't reflect X | Claim-vs-structure mismatch |
| Doc lists review criteria in one section, then visibly violates one of them elsewhere in the same doc | Self-application failure |
| The same idea appears in three non-adjacent sections with slightly different wording; no single canonical statement | Paraphrase overlap |
| "We will support X" / "The system supports X" with no concrete artifact, owner, or test | Aspirational language |
| Single doc covers project vision, technical architecture, AND operational roadmap; readers of any one role get squeezed by the other two | Scope sprawl |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
