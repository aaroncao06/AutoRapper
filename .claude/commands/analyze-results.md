---
description: Analyze results from a completed experiment
argument-hint: <experiment-id-or-plan-path>
---

# Analyze Results

## Target

Analyze experiment: `$ARGUMENTS`

This can be an experiment ID (e.g., `exp-003`) or a path to the experiment plan file.

## Process

### 1. Locate Experiment Artifacts

- Find the experiment entry in `EXPERIMENT-LOG.md`
- Read the experiment plan (from `.agents/plans/` or the path provided)
- Locate:
  - Checkpoints (best model, final model)
  - Training logs (W&B run, local logs)
  - Any saved metrics or outputs
- Verify training completed successfully
  - If training failed or is still running, report status and stop

### 2. Run Evaluation

Execute the POST-TRAINING tasks from the experiment plan:

- Load the best checkpoint
- Run evaluation scripts on all specified splits (val, test)
- Compute all metrics specified in the evaluation protocol
- Record exact numbers

If no POST-TRAINING tasks are specified in the plan, use the project's standard evaluation pipeline:
```bash
# Example — adapt to project's actual eval command:
python evaluate.py --checkpoint {checkpoint_path} --config {config_path}
```

### 3. Generate Comparisons

- Load baseline results from:
  - Previous experiment entries in EXPERIMENT-LOG.md
  - Baseline numbers from the research brief
  - Known results from literature (noted in the plan)
- Create side-by-side metric comparison table:

```markdown
| Method | Metric 1 | Metric 2 | Metric 3 |
|--------|----------|----------|----------|
| Baseline A | X.XX | X.XX | X.XX |
| Baseline B | X.XX | X.XX | X.XX |
| **Ours (exp-NNN)** | **X.XX** | **X.XX** | **X.XX** |
| Delta vs best baseline | +X.XX | +X.XX | +X.XX |
```

- Run statistical significance tests if specified in the plan (e.g., paired t-test across seeds)

### 4. Generate Visualizations

Create plots and save to `results/figures/`:

- **Training curves**: loss, metrics over epochs/steps
- **Comparison bar charts**: this experiment vs baselines
- **Ablation charts** (if this is part of an ablation study)
- **Other plots** specified in the experiment plan

Save figures with descriptive filenames:
```
results/figures/exp-NNN_{plot_type}.pdf
results/figures/exp-NNN_{plot_type}.png
```

Generate LaTeX table snippets for the paper:
```
results/tables/exp-NNN_results.tex
```

### 5. Update Experiment Log

Update the experiment's entry in `EXPERIMENT-LOG.md`:

- Change **Status** from RUNNING to COMPLETED (or FAILED)
- Fill in **Completed** date
- Fill in **Results** section with actual metric values
- Fill in **vs Baseline** with delta from best baseline
- Set **Assessment**:
  - **Supported**: Results confirm the hypothesis
  - **Refuted**: Results contradict the hypothesis
  - **Inconclusive**: Results are mixed or not statistically significant
- Update the summary table at the top

### 6. Output Report

```markdown
## Experiment Analysis: exp-NNN — {short name}

### Hypothesis
{hypothesis from plan}

### Assessment: {SUPPORTED / REFUTED / INCONCLUSIVE}

### Key Results
| Metric | Value | vs Baseline | Significant? |
|--------|-------|------------|-------------|
| {metric} | {value} | {delta} | {yes/no/N/A} |

### Generated Artifacts
- Figure: `results/figures/exp-NNN_{type}.pdf`
- Table: `results/tables/exp-NNN_results.tex`

### Implications for Paper
- {Which claim is supported/weakened}
- {What this means for the paper narrative}

### Suggested Next Steps
- {Next experiment to run, if any}
- {Claim ready for paper writing, if sufficient evidence}
- {Additional analysis needed, if any}
```

## Notes

- This command is self-contained — it re-reads the experiment plan and log rather than relying on session memory. Days may pass between `/execute` and `/analyze-results`.
- If the experiment failed, document the failure mode and suggest fixes
- If results are surprising, note this and suggest investigation
- Always use exact numbers from evaluation — never approximate
- Save all generated artifacts to `results/` with clear naming
