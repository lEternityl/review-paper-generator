# Review Paper Generator

A Claude Code skill for generating complete, publication-ready review papers from a single research topic.

## Overview

This skill orchestrates an end-to-end pipeline for academic review paper generation:

```
Research Topic → Literature Search → Evidence Extraction → Figure Generation → Drafting → LaTeX Output
```

## Features

- **Automated Literature Search**: Searches multiple academic databases (Google Scholar, Semantic Scholar, arXiv, etc.)
- **Structured Evidence Extraction**: Extracts and organizes findings from 20+ sources
- **Figure Generation**: Automatically generates taxonomy diagrams, trend charts, architecture diagrams, and comparison tables
- **Bilingual Support**: Supports both English and Chinese paper generation
- **LaTeX Template Integration**: Works with user-provided journal templates
- **Publication-Ready Output**: Generates compilable `.tex` files with proper formatting

## Pipeline Phases

| Phase | Description | Output |
|-------|-------------|--------|
| 1. Clarify | Ask user for scope, target journal, template | Requirements |
| 2. Plan | Present research plan for approval | Approved plan |
| 3. Research | Deep literature search and evidence extraction | Source ledger |
| 4. Visualize | Generate figures and tables | `figures/` directory |
| 5. Draft | Write Markdown manuscript | `draft.md` |
| 6. LaTeX | Format and insert figures into template | `main.tex` + `references.bib` |

## Installation

1. Clone this repository or download the folder
2. Place it in your Claude Code skills directory:
   ```bash
   mkdir -p ~/.claude/skills/
   cp -r review-paper-generator ~/.claude/skills/
   ```

## Usage

In Claude Code, simply describe your review paper topic:

```
"Write a review paper about AI applications in emergency management"
"帮我写一篇关于深度学习在医疗影像中应用的综述"
"Generate a survey on large language models for code generation"
```

The skill will automatically:
1. Ask clarifying questions (target journal, scope, language)
2. Present a research plan for your approval
3. Search and analyze relevant literature
4. Generate figures and tables
5. Draft the manuscript
6. Output a compilable LaTeX file

## File Structure

```
review-paper-generator/
├── SKILL.md                    # Main skill definition
├── README.md                   # This file
├── assets/
│   └── template_default.tex    # Default LaTeX template
└── references/
    ├── latex-conversion.md     # Markdown to LaTeX conversion rules
    └── paper-structures.md     # Journal-specific structure guidelines
```

## Output Files

After running the skill, you'll receive:

| File | Description |
|------|-------------|
| `main.tex` | LaTeX main file with proper formatting |
| `references.bib` | BibTeX bibliography file |
| `figures/` | Directory containing all generated figures |
| `draft.md` | Markdown draft of the manuscript |

## Compilation

To compile the output LaTeX file:

```bash
pdflatex main.tex
bibtex main
pdflatex main.tex
pdflatex main.tex
```

Or with XeLaTeX (for Chinese support):

```bash
xelatex main.tex
bibtex main
xelatex main.tex
xelatex main.tex
```

## Requirements

- Claude Code CLI installed
- LaTeX distribution (TeX Live, MiKTeX, or MacTeX)
- Python 3.x with matplotlib (for figure generation)

## Customization

### Using Your Own Template

Provide your `.tex` template file path when prompted. The skill will:
1. Read your template structure
2. Identify insertion points
3. Map content to your template's format
4. Generate a compilable document

### Supported Journal Formats

- ACM / IEEE conference formats
- Elsevier journal formats
- Springer journal formats
- Chinese journal formats (华科学报, 自然科学学报, etc.)
- Custom formats via user templates

## Example Topics

- "AI in emergency resource scheduling optimization"
- "Large language models for software engineering: A survey"
- "Graph neural networks for network intrusion detection"
- "深度强化学习在自动驾驶中的应用综述"
- "联邦学习在医疗数据隐私保护中的研究进展"

## License

MIT License

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Acknowledgments

Built with [Claude Code](https://github.com/anthropics/claude-code) by Anthropic.
