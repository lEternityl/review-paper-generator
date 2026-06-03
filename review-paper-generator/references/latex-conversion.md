# LaTeX Conversion Reference

Detailed rules for converting Markdown draft to compilable LaTeX.

## Markdown → LaTeX Mapping

### Section Headers

| Markdown | LaTeX |
|----------|-------|
| `# Title` | `\title{Title}` + `\maketitle` |
| `## Section` | `\section{Section}` |
| `### Subsection` | `\subsection{Subsection}` |
| `#### Subsubsection` | `\subsubsection{Subsubsection}` |

### Text Formatting

| Markdown | LaTeX |
|----------|-------|
| `**bold**` | `\textbf{bold}` |
| `*italic*` | `\textit{italic}` |
| `` `code` `` | `\texttt{code}` |
| `> blockquote` | `\begin{quote}...\end{quote}` |

### Lists

```markdown
- item 1            →  \begin{itemize}
- item 2                \item item 1
                        \item item 2
                    \end{itemize}

1. first            →  \begin{enumerate}
2. second               \item first
                        \item second
                    \end{enumerate}
```

### Citations

| Markdown | LaTeX (numeric) | LaTeX (author-year) |
|----------|-----------------|---------------------|
| `[1]` | `[1]` or `\cite{ref1}` | `\cite{ref1}` |
| `[1,2,3]` | `[1,2,3]` or `\cite{ref1,ref2,ref3}` | `\cite{ref1,ref2,ref3}` |

**Default approach**: Use numeric citation style `[n]` in body, with numbered `\bibitem` entries.
If the user's template uses BibTeX, generate a `.bib` file and use `\cite{key}`.

### Figures

Markdown placeholder:
```
[FIGURE:1:A comprehensive taxonomy of LLM applications in emergency management]
```

LaTeX conversion:
```latex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=\textwidth]{figures/fig_1.png}
  \caption{A comprehensive taxonomy of LLM applications in emergency management.}
  \label{fig:taxonomy}
\end{figure}
```

### Tables

Markdown placeholder:
```
[TABLE:1:Comparison of representative LLM-based emergency management systems]
```

LaTeX conversion:
```latex
\begin{table}[htbp]
  \centering
  \caption{Comparison of representative LLM-based emergency management systems.}
  \label{tab:comparison}
  \begin{tabular}{lp{3cm}p{3cm}p{3cm}}
  \toprule
  System & Features & Scenarios & Performance \\
  \midrule
  ... & ... & ... & ... \\
  \bottomrule
  \end{tabular}
\end{table}
```

## Common Package Requirements

For a compilable review paper, include these packages:

```latex
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{graphicx}       % for \includegraphics
\usepackage{hyperref}       % clickable links
\usepackage{booktabs}       % professional tables (\toprule, \midrule, \bottomrule)
\usepackage{amsmath,amssymb} % math
\usepackage[margin=1in]{geometry}
\usepackage{natbib}         % if using author-year citations
\usepackage{multirow}       % multi-row table cells
\usepackage{xcolor}         % colored text (use sparingly)
```

## BibTeX Entry Templates

### Journal Article
```bibtex
@article{key,
  author    = {Author, First and Author, Second},
  title     = {Paper Title},
  journal   = {Journal Name},
  volume    = {1},
  number    = {1},
  pages     = {1--10},
  year      = {2025},
  doi       = {10.xxxx/xxxxx},
  url       = {https://doi.org/10.xxxx/xxxxx}
}
```

### Conference Paper
```bibtex
@inproceedings{key,
  author    = {Author, First},
  title     = {Paper Title},
  booktitle = {Proceedings of Conference Name},
  year      = {2025},
  pages     = {1--10},
  doi       = {10.xxxx/xxxxx}
}
```

### arXiv Preprint
```bibtex
@article{key,
  author    = {Author, First},
  title     = {Paper Title},
  journal   = {arXiv preprint arXiv:xxxx.xxxxx},
  year      = {2025},
  url       = {https://arxiv.org/abs/xxxx.xxxxx}
}
```

### Web Resource / Technical Report
```bibtex
@misc{key,
  author    = {{Organization Name}},
  title     = {Page or Document Title},
  year      = {2025},
  url       = {https://example.com/page},
  note      = {Accessed: 2026-06-02}
}
```

## Troubleshooting Common LaTeX Errors

### "Undefined control sequence"
- Missing `\usepackage{}`. Add the required package.
- Typo in command name.

### "File 'figures/fig_1.png' not found"
- Check that the `figures/` directory exists in the same location as the `.tex` file
- Use relative paths from the .tex file location

### "Overfull \hbox"
- An element is too wide for the page. Options:
  - Reduce figure/table width: `width=0.95\textwidth`
  - Use `\resizebox{\textwidth}{!}{...}` for wide tables
  - Adjust page margins temporarily

### "Citation 'ref1' undefined"
- Run `bibtex` (or `biber`) between `pdflatex` runs
- Check that the citation key in `\cite{key}` matches the `.bib` entry
- Compilation sequence: `pdflatex → bibtex → pdflatex → pdflatex`

### CJK (Chinese/Japanese/Korean) Text
If the paper contains CJK characters:
- Use `\usepackage[UTF8]{ctex}` for Chinese documents
- Or `\usepackage{CJKutf8}` with `\begin{CJK}{UTF8}{gbsn}` for mixed language
- Ensure the LaTeX engine supports UTF-8 (use `xelatex` or `lualatex` instead of `pdflatex`)

## Compilation Instructions (to include with output)

```bash
# For English papers:
pdflatex paper.tex
bibtex paper
pdflatex paper.tex
pdflatex paper.tex

# For Chinese papers or papers with CJK:
xelatex paper.tex
bibtex paper
xelatex paper.tex
xelatex paper.tex
```
