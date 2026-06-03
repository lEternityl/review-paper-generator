---
name: review-paper-generator
description: |
  End-to-end review paper generation from a research topic. Orchestrates the full pipeline: 
  clarify scope → search literature (web_search + Google Scholar) → deep fetch and evidence 
  extraction → generate figures/tables (scientific-schematics, scientific-visualization) → 
  draft manuscript in Markdown → format into LaTeX using user-provided template → output 
  compilable .tex file. Use when the user says "write a review paper", "generate a literature 
  review", "综述论文", "写综述", "survey paper", or provides a research topic and asks for a 
  complete publishable paper output. Triggers on both English and Chinese requests.
---

# Review Paper Generator

Generate a complete, publication-ready review paper from a single research topic. This skill
orchestrates the end-to-end pipeline: literature search → evidence extraction → figure generation 
→ drafting → LaTeX formatting.

## Pipeline Overview

```
Input: Research Topic
  │
  ├─ Phase 1: CLARIFY ──── ask_user (1-3 questions) → STOP
  │
  ├─ Phase 2: PLAN ─────── present plan → wait for approval → STOP  
  │
  ├─ Phase 3: RESEARCH ──── deep-research protocol (web_search + search_scholar + web_fetch)
  │
  ├─ Phase 4: VISUALIZE ── generate figures/tables (scientific-schematics, scientific-visualization)
  │
  ├─ Phase 5: DRAFT ────── write Markdown manuscript from evidence
  │
  └─ Phase 6: LATEX ────── merge draft into user's LaTeX template → output .tex
```

## Phase 1: Clarify Scope

Before ANY tool call, ask the user 1-3 essential clarifying questions. Typical questions:

1. **Target journal/venue**: Which journal or conference? This determines length, format, and depth.
   (e.g., Nature Reviews, ACM Computing Surveys, 中文核心期刊, arXiv preprint)
2. **Scope boundaries**: Specific sub-topics to include/exclude? Time range? Language preference?
3. **LaTeX template**: Ask the user to provide their `.tex` or `.cls` template file path, OR 
   confirm they will provide it in Phase 6. If no template yet, note this and prepare to use 
   a generic `article` class as fallback.

Restate the task in your own words and wait for user response before proceeding.

## Phase 2: Research Plan

Present a concise plan to the user, including:

- **Task restatement**: the review topic, scope, target venue, and working assumptions
- **Research angles**: 4-6 sub-questions or thematic lenses that will structure the review
- **Execution plan**: 
  - Stage A: build ≥20 candidate URLs via web_search + search_scholar (6-8 rounds)
  - Stage B: deep-fetch top 10-15 URLs for full-text evidence
  - Figure plan: what types of diagrams/tables will be generated (taxonomy, timeline, architecture, 
    comparison table, etc.)
  - Draft target: approximate word count based on journal requirements
- **Deliverable shape**: A compilable `.tex` file using the user's template, with all figures 
  embedded and BibTeX references
- **Approval request**: Explicit confirmation before any research tool call

## Phase 3: Staged Literature Search

Follow the `deep-research` skill's reflective retrieval protocol:

### Stage A: Shallow Search (Candidate Building)

- Use **both** `web_search` and `search_scholar`:
  - `web_search`: for recent news, technical blogs, arXiv, preprints, government/industry reports
  - `search_scholar`: for high-impact peer-reviewed papers (set `year_start` ≥ target start year)
- Search in both English and Chinese when the user's topic is in Chinese or bilingual
- Build a candidate pool of at least **20 unique URLs** before entering Stage B
- After each search, reflect: what gap remains? which approved angle still lacks coverage?
- Revise queries using the template: `[approved angle] + [current gap] + [adjustment]`
- Default turn budget: **20 turns** for the full loop, with majority spent on Stage A

### Stage B: Deep Fetch and Validation

- Rank candidates by source quality: **peer-reviewed > official/government > industry report > preprint > news**
- Fetch top 10-15 URLs via `web_fetch`
- Extract structured evidence from each fetch:
  - **source_ledger**: title, authors, year, venue, URL, source_type
  - **evidence_cards**: claim, detail, why-it-matters, confidence (HIGH/MEDIUM/LOW)
- If fetched content reveals a gap, return to Stage A with a revised query

### Stop Conditions

Stop when:
- Core claims are supported by ≥3 strong sources
- No major information gap remains
- Candidate pool and fetched evidence cover all approved research angles

## Phase 4: Figure and Table Generation

Before drafting, plan and generate all visual elements:

### 4.1 Figure Types

| Figure Type | Tool to Use | Example |
|-------------|-------------|---------|
| Taxonomy / classification diagram | `generate_image` (via scientific-schematics) | "Hierarchical taxonomy of LLM applications in emergency management" |
| System architecture | `generate_image` | "End-to-end pipeline of the proposed framework" |
| Timeline / evolution | `generate_image` | "Evolution of key methods from 2020 to 2026" |
| Comparison table | `execute_code` (Python tabulate/pandas) → LaTeX table | "Comparison of 10 representative methods" |
| Statistical chart | `scientific-visualization` (matplotlib) | "Publication trend by year" |

### 4.2 Figure Generation Workflow

1. **Plan figures**: List every figure/table needed based on the evidence structure
2. **Generate each figure** using the appropriate tool:
   - For schematic/diagram: call `generate_image` with a detailed English prompt
   - For data charts: write Python code to produce publication-quality `.png` via matplotlib
   - For tables: generate LaTeX-formatted tables
3. **Save all figures** to a `figures/` subdirectory in the task working directory
4. **Quality check**: verify readability, colorblind-friendliness, and consistency

### 4.3 Prompt Template for generate_image

```
"[diagram type] showing [content description]. [Visual style requirements].
Clean white background, professional typography, colorblind-friendly Okabe-Ito palette.
All labels readable at print size. [Aspect ratio if needed]."
```

## Phase 5: Manuscript Drafting

Draft the review paper in Markdown before converting to LaTeX:

### 5.1 Paper Structure

Follow standard review paper IMRaD-adapted structure:

1. **Title** — Concise, informative
2. **Abstract** — 150-250 words: background, scope, key findings, implications
3. **Introduction** — Motivation, scope, contributions, paper organization
4. **Background / Preliminaries** — Foundational concepts, taxonomy definitions
5. **Core Analysis Sections** — 4-6 thematic sections based on research angles
6. **Discussion** — Synthesis, trends, open challenges
7. **Conclusion** — Summary, limitations, future directions
8. **References** — Numbered list, all with clickable URLs

### 5.2 Drafting Rules

- Target length: **6,000-10,000 words** for a standard survey; adjust per journal requirements
- Use academic English (or Chinese) appropriate for the target venue
- Every substantive claim must have a citation `[n]`
- Number citations in order of first appearance
- Maintain source_ledger throughout drafting for citation mapping
- Use `[FIGURE:n:caption]` and `[TABLE:n:caption]` as placeholders that will be replaced
  with LaTeX `\includegraphics` and `\begin{table}` during Phase 6
- **Note**: These placeholders MUST be converted to actual LaTeX figure environments in Phase 6
- Each major section: ≥3 substantive paragraphs
- Avoid: thin bullet-heavy sections, uncited analytical paragraphs, vague attributions

### 5.3 Quality Gate (Before LaTeX Conversion)

Verify:
- [ ] All citations map to source_ledger entries
- [ ] Every source_ledger entry has author, year, title, URL
- [ ] No uncited sources in reference list
- [ ] All planned figures/tables are referenced in text
- [ ] Each major section has ≥3 paragraphs
- [ ] Abstract captures scope, findings, and implications

## Phase 6: LaTeX Formatting

Convert the Markdown draft into a compilable `.tex` file using the user's template.

### 6.1 Template Integration

1. **Read user's template**: Load the `.tex` or `.cls` template file provided in Phase 1
2. **Identify insertion points**: Find `\begin{document}`, `\title{}`, `\author{}`, `\section{}` patterns
3. **Map Markdown to LaTeX**:
   - `# Title` → `\title{...}` + `\maketitle`
   - `## Section` → `\section{...}`
   - `### Subsection` → `\subsection{...}`
   - `**bold**` → `\textbf{...}`
   - `*italic*` → `\textit{...}`
   - `[n]` citation → `\cite{ref_n}` or `[n]` (depending on template)
   - `[FIGURE:n:caption]` → `\begin{figure}...\includegraphics{figures/fig_n.png}...\end{figure}`
   - `[TABLE:n:caption]` → `\begin{table}...\end{table}`
   - Bullet lists → `\begin{itemize}...\end{itemize}`
   - Numbered lists → `\begin{enumerate}...\end{enumerate}`
4. **Generate BibTeX**: Create `references.bib` from source_ledger entries

### 6.2 Insert Figures into LaTeX

**IMPORTANT**: After generating the `.tex` file, you MUST explicitly insert all figures using `\begin{figure}` environments. Do NOT rely solely on placeholders.

1. **Identify insertion points**: Find appropriate locations in the text where each figure should appear (usually after the first reference to the figure)
2. **Insert figure environments** using this template:
   ```latex
   \begin{figure}[H]
       \centering
       \includegraphics[width=0.9\textwidth]{figures/fig_name.png}
       \caption{中文标题（English caption）}
       \label{fig:label_name}
   \end{figure}
   ```
3. **Add figure references in text**: Ensure each figure is referenced in the body text using `\ref{fig:label_name}`
4. **Typical figure placement**:
   - Taxonomy/classification figure: after Introduction or in Background section
   - Trend/timeline figure: after literature review section
   - Architecture figure: in methodology/framework section
   - Distribution/comparison figures: in analysis or discussion section

### 6.3 Output

- A single compilable `.tex` file (or main `.tex` + separate `references.bib`)
- A `figures/` directory with all generated images
- **Verify all figures are included**: Run compilation and check PDF output
- Instructions for compilation (e.g., `pdflatex → bibtex → pdflatex × 2`)

### 6.4 Fallback (No Template Provided)

If the user has not provided a template by Phase 6:
- Use `\documentclass[review]{article}` with standard ACM or IEEE formatting
- Include necessary packages: `graphicx`, `cite`, `hyperref`, `booktabs`, `amsmath`
- Generate a clean, compilable standalone `.tex` file

## Tool Usage Policy

This skill orchestrates the following tools. Load only when their phase is reached:

| Phase | Primary Tools | Supplementary |
|-------|---------------|---------------|
| 1. Clarify | `ask_user` | — |
| 2. Plan | `ask_user` | — |
| 3. Research | `web_search`, `search_scholar`, `web_fetch` | `read` (for user's template) |
| 4. Visualize | `generate_image`, `execute_code` | `scientific-visualization` skill |
| 5. Draft | `execute_code` (to write .md) | — |
| 6. LaTeX | `read` (template), `execute_code` (to write .tex) | `write` |

## References

Load these as needed during execution:
- `references/latex-conversion.md`: Detailed Markdown→LaTeX conversion rules, common package usage, 
  and troubleshooting compilation errors
- `references/paper-structures.md`: Journal-specific structure requirements and word count 
  guidelines for major venues

## Minimum Requirements

- Preserve Phase 1 and Phase 2 user stop points — never proceed without explicit approval
- Use reflective search/fetch loop with ≥20 candidate URLs before deep fetch
- Generate at least 2-4 figures/tables unless user requests otherwise
- Draft ≥6,000 words before LaTeX conversion
- All external claims must be cited
- Output a compilable `.tex` file
- **CRITICAL**: All generated figures MUST be explicitly inserted into the `.tex` file using `\begin{figure}` environments
- Verify figures appear in compiled PDF before delivering final output
- Package all generated files (`.tex`, `.bib`, `figures/`) into a single `.tar.gz`
