---
description: Conduct a structured literature review on a topic
argument-hint: <topic-or-query>
---

# Literature Review

## Topic

`$ARGUMENTS`

## Objective

Conduct a systematic literature review: find relevant papers, extract key results and methods, identify baselines, and produce a structured reference document. This is useful during research planning, when preparing the related work section, or when seeking baseline numbers for experiment comparison.

## Process

### 1. Search for Papers

Use multiple sources to find relevant papers:

**Web Search:**
- Search for the topic on Google Scholar, arXiv, and general web
- Use queries like: "{topic} arxiv 2024 2025", "{topic} state of the art", "{topic} benchmark results"

**Semantic Scholar API** (via WebFetch):
```
https://api.semanticscholar.org/graph/v1/paper/search?query={topic}&limit=20&fields=title,authors,year,abstract,citationCount,venue,externalIds,openAccessPdf
```

**Follow citation chains:**
- For highly relevant papers, check their references and "cited by" lists
- Look for survey papers on the topic — they aggregate baselines and methods

### 2. Filter and Prioritize

From search results, select the most relevant papers based on:
- **Recency**: Prefer papers from the last 2-3 years
- **Relevance**: Directly addresses the same problem or method
- **Impact**: High citation count or published at top venues (NeurIPS, ICML, ICLR, ACL, CVPR, etc.)
- **Baselines**: Papers that report numbers on the same benchmarks we use

Aim for 10-20 papers, organized into tiers:
- **Must-cite** (5-8): Directly comparable work, foundational methods
- **Should-cite** (5-8): Related approaches, broader context
- **Awareness** (3-5): Tangentially related, useful for framing

### 3. Extract Information Per Paper

For each selected paper, extract:

```markdown
### {Title} ({Authors}, {Year})
- **Venue**: {conference/journal}
- **Link**: {arXiv or DOI URL}
- **Method**: {1-2 sentence summary of approach}
- **Key Results**:
  - {Dataset}: {Metric} = {Value}
  - {Dataset}: {Metric} = {Value}
- **Relevance**: {Why this matters to our work}
- **Difference from ours**: {How our approach differs}
```

### 4. Organize by Research Themes

Group papers into themes that would form paragraphs in a Related Work section. For example:
- Theme 1: {Approach category A} — papers X, Y, Z
- Theme 2: {Approach category B} — papers A, B, C
- Theme 3: {Application domain} — papers D, E

For each theme, note:
- What the common approach is
- What limitations exist (the gap our work addresses)

### 5. Extract Baseline Table

Create a comparison table of reported results:

```markdown
## Baseline Results

| Method | Year | {Dataset 1} {Metric} | {Dataset 2} {Metric} | Notes |
|--------|------|----------------------|----------------------|-------|
| {Method A} | 2024 | {value} | {value} | {brief note} |
| {Method B} | 2023 | {value} | {value} | {brief note} |
```

Note any caveats: different eval protocols, different data splits, different preprocessing.

### 6. Output

Write the structured review to: `.agents/reference/lit-review-{kebab-case-topic}.md`

The output file should contain:
1. **Search Summary** — queries used, number of papers found, date of review
2. **Paper Summaries** — organized by theme (from step 3 & 4)
3. **Baseline Table** — comparative results (from step 5)
4. **Research Gaps** — what's missing in the literature that our work addresses
5. **Suggested Citations** — BibTeX entries for must-cite papers (if available)

### 7. Suggest Updates

After writing the review, suggest updates to:
- **RESEARCH-BRIEF.md** — Related Work Summary section (if it exists)
- **RESEARCH-BRIEF.md** — Baseline numbers in Evaluation Protocol
- **`paper/references.bib`** — new BibTeX entries to add

Do NOT make these updates automatically — present them as suggestions for the user to approve.

## Notes

- The Semantic Scholar API is free and doesn't require authentication for basic queries
- For papers behind paywalls, extract what you can from abstracts, arXiv versions, and blog posts
- Always verify numbers — don't hallucinate baseline results. If you can't find exact numbers, say "not found" rather than guessing
- This command can be run multiple times on different subtopics
- Cross-reference with papers already cited in RESEARCH-BRIEF.md to avoid duplicating effort
- If the project has a `paper/references.bib`, check it to see what's already been cited
