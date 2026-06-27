# Layout Debugging Workflow

Use this reference when the user provides PDF screenshots or asks to adjust
paper layout.

## First Pass

Identify the visible problem before editing:

- figure too wide
- table overflowing a column
- equation too long
- float placed awkwardly
- caption or label issue
- reference formatting problem
- final page columns visibly unbalanced

## Fix Strategy

Prefer local fixes:

- reduce figure width with `width=\linewidth` or a smaller fraction
- simplify or resize table columns
- split long equations across lines
- adjust float placement with `[t]`, `[tbp]`, `figure*`, or `table*`
- rewrite overly long captions or table headers
- move large figures/tables near their first citation

## Avoid

Do not change IEEEtran global layout, margins, fonts, column widths, or class
file internals. Do not use absolute image paths.

## Verify

Compile after layout edits. If visual judgment is required, ask the user for a
fresh screenshot or inspect the generated PDF when tooling allows it.
