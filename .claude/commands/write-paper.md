---
description: Write or update a paper section from experiment results
argument-hint: <section-name>
---

# Write Paper Section

## Target Section

Write or update: `$ARGUMENTS`

Valid section names: `abstract`, `introduction`, `method`, `experiments`, `results`, `related-work`, `conclusion`, `all`

## Process

### 1. Gather Context

Read all relevant sources:

- **RESEARCH-BRIEF.md** — claims, hypotheses, scope, target venue
- **EXPERIMENT-LOG.md** — all completed experiments, their results and assessments
- **Existing paper sections** in `paper/` — what's already written
- **Results artifacts** — figures in `results/figures/`, tables in `results/tables/`
- **CLAUDE.md** — paper conventions, compilation command

### 2. Evidence Mapping

Before writing, create a mapping:

```markdown
## Evidence Map

### Claim 1: {claim statement}
- Supported by: exp-001 ({metric}: {value}), exp-003 ({metric}: {value})
- Figures: results/figures/exp-001_comparison.pdf
- Tables: results/tables/exp-001_results.tex
- Strength: {STRONG / MODERATE / WEAK / INSUFFICIENT}

### Claim 2: {claim statement}
- Supported by: exp-002 ({metric}: {value})
- MISSING: {what additional evidence is needed}
- Strength: {STRONG / MODERATE / WEAK / INSUFFICIENT}
```

Flag any:
- Claims without sufficient evidence
- Claims that should be weakened based on actual results
- Results that suggest new claims not in the original brief

### 3. Write/Update Section

Write directly to LaTeX files in `paper/`:
- Main file: `paper/main.tex`
- Section files: `paper/sections/{section}.tex` (if the project uses split files)

**Writing guidelines:**

- Pull exact numbers from experiment results — never approximate or guess
- Reference figures and tables using LaTeX `\ref{}` and `\cite{}`
- Follow academic conventions for the target venue
- Use precise scientific language
- Include quantitative comparisons with baselines
- Note statistical significance where applicable

**Section-specific guidance:**

**Abstract**: 150-250 words. Problem, approach, key result (with number), implication.

**Introduction**: Motivate the problem, state the gap, describe approach, summarize contributions (numbered list), outline paper structure.

**Related Work**: Organize by research themes (not chronologically). Position this work relative to each theme. Use `\cite{}` references.

**Method**: Describe approach precisely enough to reproduce. Include model architecture, training procedure, key design choices with justification.

**Experiments**: Describe experimental setup — datasets, metrics, baselines, implementation details (hyperparameters, compute). Reference config files for full details.

**Results**: Present results with tables and figures. Compare against baselines. Discuss statistical significance. Include ablation studies. Discuss what worked and what didn't.

**Conclusion**: Summarize contributions, acknowledge limitations, suggest future work.

### 4. Output

After writing:

1. Confirm which files were created/modified with full paths
2. Show the evidence map
3. List any **missing evidence** — claims that need more experiments
4. List any **suggested figures/tables** to create (run `/analyze-results` if needed)
5. Flag if the paper narrative needs adjustment based on actual results
6. If applicable, try to compile the paper:
   ```bash
   cd paper && latexmk -pdf main.tex
   ```

## Notes

- If writing `all`, write sections in this order: method, experiments, results, introduction, related-work, conclusion, abstract (abstract last because it summarizes everything)
- Never fabricate or approximate results — use exact numbers from EXPERIMENT-LOG.md and results files
- If evidence is insufficient for a claim, say so explicitly rather than overstating results
- Keep the target venue's conventions in mind (page limits, formatting, citation style)
- For related work, add entries to `paper/references.bib` as needed
- This command can be run multiple times — it updates existing sections rather than rewriting from scratch
