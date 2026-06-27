# ICUS Paper Workspace

这是一个本地 VS Code + LaTeX Workshop + GitHub + GitHub Actions 的 IEEE 会议论文写作工作区。

## 日常写作

1. 用 VS Code 打开 `D:\PycharmProjects\ICUS`。
2. 编辑 `main.tex`。
3. 保存 `main.tex` 后，LaTeX Workshop 会自动编译。
4. 在 VS Code 右上角或 LaTeX Workshop 面板中打开 PDF 预览。

## 参考文献

参考文献写在 `refs.bib` 里。正文中用 `\cite{文献key}` 引用，例如：

```tex
A sample citation looks like this \cite{placeholder_article}.
```

每条文献都有一个 key，例如 `placeholder_article`。以后从 Google Scholar、IEEE Xplore、arXiv 或 Zotero 导出 BibTeX 时，把条目追加到 `refs.bib`，再在正文里引用对应 key。

## 图片

建议把图片放进 `figures/` 文件夹，然后在 `main.tex` 中这样引用：

```tex
\begin{figure}[t]
  \centering
  \includegraphics[width=\linewidth]{figures/example.png}
  \caption{Your figure caption.}
  \label{fig:example}
\end{figure}
```

正文引用图片时使用 `Fig.~\ref{fig:example}`。

## GitHub Actions

每次把代码推送到 GitHub 的 `main` 分支后，GitHub Actions 会在云端编译 `main.tex`，并把生成的 `main.pdf` 作为 artifact 保存。

本地 VS Code 编译用于日常写作和预览；GitHub Actions 编译用于确认仓库在干净环境里也能生成 PDF。
