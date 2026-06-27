# AGENTS.md

This repository is an IEEE/ICUS LaTeX paper workspace. Codex should help with
paper writing, LaTeX fixes, bibliography maintenance, layout debugging, local
build verification, and Git hygiene.

## Key Files

- `main.tex`: paper source and main editing target.
- `refs.bib`: BibTeX bibliography database.
- `IEEEtran.cls`: IEEE class file; do not modify unless explicitly requested.
- `IEEEtran.bst`: IEEE BibTeX style; do not modify unless explicitly requested.
- `.vscode/settings.json`: VS Code / LaTeX Workshop local build settings.
- `.github/workflows/latex.yml`: GitHub Actions PDF build workflow.

## Hard Rules

- Work from `D:\PycharmProjects\ICUS` for this paper.
- Preserve IEEEtran formatting; do not change margins, fonts, column widths, or
  template internals unless the user explicitly asks.
- Do not invent paper results, experiments, citations, author details, or claims.
- Do not commit generated LaTeX outputs such as `main.pdf`, `.aux`, `.bbl`,
  `.blg`, `.log`, `.fls`, `.fdb_latexmk`, or `.synctex.gz`.
- Keep edits narrow and related to the user request.
- The user is not a LaTeX expert; fix build errors directly when feasible and
  explain the result in plain Chinese.

## Local Build

Use TeX Live 2026 explicitly; do not rely on `PATH`.

```powershell
& 'D:\latex\texlive\2026\bin\windows\latexmk.exe' -pdf -interaction=nonstopmode -synctex=1 -file-line-error main.tex
```

Run a local build after changes to `main.tex`, `refs.bib`, figures, LaTeX
settings, or GitHub Actions. A successful build must produce `main.pdf` with
exit code 0.

Clean only when needed to verify a fresh build:

```powershell
& 'D:\latex\texlive\2026\bin\windows\latexmk.exe' -C main.tex
```

## Bibliography

- Use BibTeX only: entries go in `refs.bib`, citations use `\cite{key}`.
- Keep these lines in `main.tex`:

```tex
\bibliographystyle{IEEEtran}
\bibliography{refs}
```

- Do not switch back to manual `thebibliography` unless the user requests it.
- Use stable ASCII citation keys, e.g. `smith2025control`; avoid spaces,
  Chinese characters, and duplicate keys.
- Preserve capitalization for terms like `{IEEE}`, `{LiDAR}`, and `{SLAM}`.

## Figures And Tables

- Put paper figures under `figures/` and reference them with relative paths.
- Do not use absolute local paths in LaTeX source.
- Prefer local layout fixes over global template changes.

## Git

- Before committing, check `git status --short --ignored`.
- Commit source/config changes only; generated PDFs and build artifacts should
  remain ignored.
- Do not use destructive Git commands such as `git reset --hard` or
  `git checkout -- .` unless the user explicitly requests them.

## When Unsure

Read the relevant parts of `main.tex`, `refs.bib`, and build logs before
changing files. If a task grows beyond these rules, create or consult a
separate workflow document rather than expanding this file.
