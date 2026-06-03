# Paper Structure Guidelines by Venue Type

Standard review paper structures for major publication venues.

## General Review Paper Structure

All review papers should follow this high-level organization:

```
1. Title & Abstract
2. Introduction (motivation, scope, contributions, organization)
3. Background / Preliminaries (taxonomy, key concepts)
4. Core Analysis (4-6 thematic sections based on research angles)
5. Discussion (synthesis, trends, open challenges)
6. Conclusion (summary, limitations, future work)
7. References
```

## Venue-Specific Guidelines

### ACM Computing Surveys

- **Length**: 10,000-20,000 words (typically 30-50 pages)
- **Abstract**: 150-250 words
- **Structure**: Standard IMRaD-adapted
- **Citation style**: Numeric [n]
- **Figures**: Embedded in text, high-resolution
- **Key requirement**: Must be a comprehensive, authoritative survey of a well-defined area
- **Audience**: Broad CS research community

### Nature Reviews (and Nature family)

- **Length**: 5,000-8,000 words (typically 8-12 pages)
- **Abstract**: 100-150 words, stand-alone
- **Structure**: 
  - Introduction (no heading)
  - Thematic sections (3-5)
  - Outlook / Future directions
- **Citation style**: Numeric superscript
- **Figures**: Commissioned illustrations, high visual quality
- **Key requirement**: Must be accessible to a broad scientific audience
- **LaTeX**: Usually use `nature` class

### IEEE Access / IEEE Transactions

- **Length**: 8-12 pages (two-column format)
- **Abstract**: 150-250 words
- **Structure**: Standard IMRaD with sections
- **Citation style**: Numeric [n]
- **Template**: `\documentclass{IEEEtran}`
- **Key requirement**: Technical depth with practical relevance

### 中文核心期刊 (Chinese Core Journals)

- **Length**: 8,000-15,000 字 (Chinese characters)
- **结构**:
  - 题目、摘要（中英文）、关键词
  - 引言
  - 相关背景/基本概念
  - 核心分析章节（按研究角度划分）
  - 讨论与展望
  - 结论
  - 参考文献
- **引用格式**: 顺序编码制 [n] 或 著者-出版年制
- **模板**: 各期刊自有的 `.cls` 文件
- **特别注意**: 中英文摘要都需要，图表标题需中英文对照

### arXiv Preprint

- **Length**: Flexible, typically 10-20 pages
- **Structure**: Any reasonable academic structure
- **Citation style**: Any consistent style
- **Template**: Usually `\documentclass{article}` with standard packages
- **Key advantage**: No length restrictions, can include supplementary material

## Word Count Targets

| Venue | Min Words | Max Words | Notes |
|-------|-----------|-----------|-------|
| ACM Computing Surveys | 10,000 | 20,000 | Comprehensive |
| Nature Reviews | 5,000 | 8,000 | Concise, accessible |
| IEEE Access | 4,000 | 8,000 | Two-column |
| 中文核心期刊 | 8,000 字 | 15,000 字 | Chinese text |
| arXiv preprint | 6,000 | 15,000 | Flexible |
| Generic survey (default) | 6,000 | 10,000 | Good for most purposes |

## Section Depth Requirements

### Introduction (800-1,200 words)
- Hook: why this topic matters now
- Background: 2-3 paragraphs establishing context
- Gap: what existing surveys miss
- Contributions: bullet or numbered list of this paper's contributions
- Organization: brief roadmap of remaining sections

### Background (800-1,500 words)
- Key definitions and taxonomies
- Historical context (if relevant)
- Notational conventions
- Scope boundaries explicitly stated

### Core Analysis Sections (each 800-2,000 words)
- Start with a mini-introduction (1 paragraph) explaining the section's focus
- Present evidence organized by theme, method, or chronology
- Include ≥1 figure or table per major section
- End with a mini-summary (1 paragraph) of key takeaways

### Discussion (800-1,500 words)
- Cross-cutting trends across all core sections
- Contradictions or tensions in the literature
- Open challenges
- Promising future directions

### Conclusion (300-500 words)
- Restate main findings
- Acknowledge limitations
- Suggest specific future work
