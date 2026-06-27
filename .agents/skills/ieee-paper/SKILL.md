---
name: ieee-paper
description: Use when editing, compiling, debugging, polishing, formatting, adding figures or tables, or managing BibTeX references for this IEEE/ICUS LaTeX paper in D:\PycharmProjects\ICUS.
---

# IEEE Paper Skill

Use this skill for work on `main.tex`, `refs.bib`, paper figures/tables,
LaTeX build errors, PDF layout issues, academic polishing, and GitHub Actions
LaTeX build checks.

## Required Start

Read `AGENTS.md` first. Keep its project rules authoritative.

## Core Workflow

1. Inspect the relevant source files before editing.
2. Make narrow, task-focused edits.
3. Compile locally with TeX Live 2026 after LaTeX, BibTeX, figure, or workflow
   changes.
4. Fix build errors directly when feasible.
5. Report the change and whether `main.pdf` was generated successfully.

## Reference Routing

Read only the relevant reference file:

- `references/latex-paper-workflow.md`: local builds, VS Code preview, clean
  builds, and GitHub Actions.
- `references/bibliography-workflow.md`: BibTeX entries, citation keys,
  capitalization protection, and reference debugging.
- `references/layout-debugging.md`: PDF screenshots, figure/table/equation
  overflow, float placement, and IEEE-safe layout fixes.

If a reference file does not cover the task, use `AGENTS.md`, project context,
and the existing IEEE template conventions.
