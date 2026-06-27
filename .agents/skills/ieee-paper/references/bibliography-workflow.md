# Bibliography Workflow

Use this reference when adding, fixing, or debugging citations and references.

## Source Of Truth

All references live in `refs.bib`. Cite them from `main.tex` with:

```tex
\cite{key}
```

Keep the bibliography block in `main.tex`:

```tex
\bibliographystyle{IEEEtran}
\bibliography{refs}
```

Do not switch to manual `thebibliography` unless the user requests it.

## Adding References

Accept BibTeX from Google Scholar, IEEE Xplore, arXiv, Zotero, or publisher
pages, then clean obvious syntax issues before compiling.

Use stable ASCII citation keys:

- `smith2025control`
- `zhang2024uav`
- `wang2026navigation`

Avoid spaces, Chinese characters, punctuation-heavy keys, and duplicate keys.

## IEEE Capitalization

Protect technical terms that must stay uppercase:

```bibtex
title = {A method for {IEEE} conference papers using {LiDAR} and {SLAM}},
```

Common protected terms include `{IEEE}`, `{ICUS}`, `{UAV}`, `{LiDAR}`, `{SLAM}`,
and named datasets or systems.

## Debugging

If a citation is undefined:

1. Check the key spelling in `main.tex`.
2. Check that `refs.bib` contains the same key.
3. Check for BibTeX syntax errors such as missing commas or braces.
4. Rebuild with `latexmk`.
