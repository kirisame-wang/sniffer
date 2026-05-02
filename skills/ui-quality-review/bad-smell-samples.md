# Bad Smell Samples — ui-quality-review

Catalog of noteworthy patterns this skill recognizes for UI-quality-specific smells. **Generic patterns** — no project memoirs.

The catalog supports per-category recognition in evidence anchors (a screen name, a component name, a frame reference, a section). Whether multiple smells cluster on the same anchor (a concentration that warrants Layer 1) is a **runtime judgment based on the actual UI artifact under review** — not a pre-authored combination.

| Pattern | Smell category |
|---|---|
| Image placed in mockup with no alt text or aria-label specification | Accessibility gap |
| Interactive button has no focus-visible style; keyboard tab order undocumented | Accessibility gap |
| Text color and background contrast ratio not stated; small text on tinted background | Accessibility gap |
| Mockup shows desktop layout only; no specification for tablet or mobile width | Responsive breakpoint silence |
| Breakpoints listed but content reflow rules omitted (does the sidebar collapse, or stack?) | Responsive breakpoint silence |
| Screen fetches data but only the success state is mocked — no loading skeleton, no error fallback, no empty list | State coverage gap |
| Form submission screen lacks pending state (button disabled? spinner? optimistic update?) | State coverage gap |
| Button rendered in default state only; no hover, active, focus, or disabled variants specified | Interaction feedback silence |
| Drag-and-drop area shows static layout; no spec for drag-over highlight, drop preview, or rejection feedback | Interaction feedback silence |
| "Primary button" appears with two different paddings and corner radii on adjacent screens | Component reuse drift |
| Card component uses 16px gap on one screen and 12px on another for the same data layout | Component reuse drift |
| Search input shows `Search...` placeholder but no error copy for "no results", "rate-limited", or empty input | Microcopy gap |
| Confirmation dialog shows `[Confirm] [Cancel]` with no spec for the action verb's specificity ("Delete forever", "Save draft") | Microcopy gap |
| Email input has format hint but no rule for length, error placement, or when validation fires (blur? submit? keystroke?) | Form validation unspecified |
| Form-level error shown at top of page, but field-level error placement is undefined | Form validation unspecified |
| Three buttons on the same screen share the same color, size, and weight — primary action not visually elevated | Visual hierarchy unclear |
| Screen has no spacing rationale; gaps between sections range 8 / 16 / 24 / 32 with no system | Visual hierarchy unclear |
| Icon placed inline with no size, format (svg/png), 1x/2x density, or source asset reference | Asset specification incomplete |
| Hero illustration referenced by name only; no dimensions, file format, or color-mode (light/dark) variants | Asset specification incomplete |
| All copy is hard-coded English; no externalization key, no plan for localization, no RTL mirroring note | Internationalization silence |
| Date and number formats hard-coded as `MM/DD/YYYY` and `1,000.00` with no locale-aware spec | Internationalization silence |

Contributions take the form of new rows in this table: one generic pattern + one matching category per row.
