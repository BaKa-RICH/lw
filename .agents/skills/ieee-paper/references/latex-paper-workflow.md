# LaTeX Paper Workflow

Use this reference for local compilation, VS Code preview, clean builds, and
GitHub Actions behavior.

## Local Build

Always call TeX Live 2026 explicitly:

```powershell
& 'D:\latex\texlive\2026\bin\windows\latexmk.exe' -pdf -interaction=nonstopmode -synctex=1 -file-line-error main.tex
```

Run this from `D:\PycharmProjects\ICUS`.

## When To Build

Build after editing:

- `main.tex`
- `refs.bib`
- figures or figure paths
- `.vscode/settings.json`
- `.github/workflows/latex.yml`

Build success means exit code 0 and a generated `main.pdf`.

## Clean Build

Use a clean build only when stale artifacts may hide a problem:

```powershell
& 'D:\latex\texlive\2026\bin\windows\latexmk.exe' -C main.tex
```

Then run the normal build command again.

## VS Code Preview

The user writes in VS Code with LaTeX Workshop. The project settings should
call `D:/latex/texlive/2026/bin/windows/latexmk.exe`, not rely on `PATH`.
The PDF preview is `main.pdf`.

## GitHub Actions

Local builds are for writing and preview. GitHub Actions is the remote
reproducibility check after push. Keep `.github/workflows/latex.yml` focused on
compiling `main.tex` and uploading the PDF artifact.
