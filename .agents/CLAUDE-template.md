# CLAUDE.md Template

A flexible template for creating global rules for ML/DL research projects. Adapt sections based on your project.

---

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

<!-- What is this research project? One paragraph description -->

{Project description: research question, approach, and contribution}

---

## Research Stack

<!-- List technologies used. Add/remove rows as needed -->

| Technology | Purpose |
|------------|---------|
| {ML framework, e.g. PyTorch} | {Model training and evaluation} |
| {Experiment tracking, e.g. W&B} | {Logging metrics and runs} |
| {Compute, e.g. single A100} | {Training infrastructure} |
| {Paper tools, e.g. LaTeX/latexmk} | {Paper compilation} |

---

## Commands

<!-- Common commands for this project. Adjust based on your setup -->

```bash
# Training
{train-command, e.g. python train.py --config configs/default.yaml}

# Evaluation
{eval-command, e.g. python evaluate.py --checkpoint checkpoints/best.pt}

# Plot Results
{plot-command, e.g. python scripts/plot_results.py}

# Compile Paper
{paper-command, e.g. cd paper && latexmk -pdf main.tex}

# Lint
{lint-command, e.g. ruff check .}
```

---

## Project Structure

<!-- Describe your folder organization -->

```
{root}/
├── configs/       # Experiment configuration YAML files
├── models/        # Model architecture definitions
├── data/          # Data loading, preprocessing, datasets
├── scripts/       # Training, evaluation, plotting scripts
├── results/       # Experiment outputs
│   ├── figures/   # Generated plots and visualizations
│   └── tables/    # Generated LaTeX tables
├── paper/         # LaTeX paper source
├── checkpoints/   # Model checkpoints (gitignored)
└── {dir}/         # {description}
```

---

## Experiment Architecture

<!-- Describe the experiment pipeline and data flow -->

{Describe the config-driven pipeline: how configs drive training, how results are stored, how evaluation works. Example:
- YAML config specifies model, data, training hyperparameters
- train.py reads config, trains model, logs to W&B, saves checkpoints
- evaluate.py loads checkpoint, runs eval, writes metrics to results/
- Plotting scripts read results/ and generate figures/}

---

## Code Patterns

<!-- Key patterns and conventions used in this codebase -->

### Config Management
- {How configs are structured, e.g. standalone YAML files in configs/, one per experiment}
- {How configs are loaded, e.g. yaml.safe_load or Hydra}

### Model Definition
- {How models are defined, e.g. nn.Module subclasses in models/}
- {Registration pattern if any}

### Training Loop
- {Training approach, e.g. custom loop, PyTorch Lightning Trainer, HuggingFace Trainer}
- {Key conventions: gradient accumulation, mixed precision, etc.}

### Metric Logging
- {How metrics are logged, e.g. wandb.log(), TensorBoard, CSV}
- {Naming conventions for metrics}

### Naming Conventions
- {File and variable naming patterns, e.g. snake_case for Python}

### Error Handling & Defensive Coding

**FAIL LOUDLY. Never silently accept unexpected state.**

- **Fail-fast principle**: If a function receives input it doesn't expect, it should crash immediately with a clear error — NOT silently handle it, return a default, or log a warning and continue. Silent failures hide bugs and produce wrong results that waste GPU hours.
- **Only handle expected cases**: Each function should explicitly handle the cases it's designed for. If an unexpected case arises, let it raise an error. Do NOT add catch-all `except Exception` blocks or generic fallbacks that mask problems.
- **No silent coercion**: Never silently convert types, clamp values to valid ranges, or substitute defaults for invalid inputs. If a value is wrong, crash.

### Assertions

**Use PLENTY of `assert` statements. They are free documentation and free bug detection.**

Assertions should be used liberally throughout the codebase to make invalid states impossible beyond that point in the code. They serve as executable documentation of assumptions and catch bugs at the earliest possible moment.

**Where to assert (non-exhaustive — add more, not fewer):**

```python
# Tensor shapes — after every reshape, view, concatenation, or dimension change
assert x.shape == (batch_size, seq_len, hidden_dim), f"Expected {(batch_size, seq_len, hidden_dim)}, got {x.shape}"

# Tensor values — NaN/Inf checks after critical operations
assert torch.isfinite(loss).all(), f"Non-finite loss: {loss.item()}"

# Config values — at load time, verify all assumptions
assert config.learning_rate > 0, f"Learning rate must be positive, got {config.learning_rate}"
assert config.num_layers >= 1, f"Need at least 1 layer, got {config.num_layers}"

# Data pipeline — verify shapes, dtypes, value ranges after loading
assert batch["input_ids"].dtype == torch.long, f"Expected long tensor, got {batch['input_ids'].dtype}"
assert batch["labels"].min() >= 0, f"Negative label found: {batch['labels'].min()}"

# Pre/post conditions — at function entry and exit
def compute_attention(q, k, v):
    assert q.dim() == 3, f"Query must be 3D, got {q.dim()}D"
    assert q.size(-1) == k.size(-1), f"Q/K dim mismatch: {q.size(-1)} vs {k.size(-1)}"
    # ... implementation ...
    assert output.shape == q.shape, f"Output shape mismatch: {output.shape} vs {q.shape}"
    return output

# State transitions — verify invariants after mutations
assert len(self.layers) == self.config.num_layers
assert all(p.requires_grad for p in self.trainable_params())
```

**Rules:**
- Always include the actual value in the assertion message (not just "wrong shape" but `f"Expected {expected}, got {actual}"`)
- Assert at function boundaries (entry and exit), not just internally
- Assert after deserialization (loading configs, checkpoints, data)
- Assert tensor shapes after every non-trivial operation
- Assertions can be stripped in production with `python -O`, so they have zero runtime cost when not needed — this means there is NO reason to be conservative with them

---

## Evaluation

<!-- How to run evaluations and what patterns to follow -->

- **Run evaluation**: `{eval-command}`
- **Metrics location**: `{results-directory}`
- **Baseline comparisons**: {how baselines are stored and compared}

---

## Experiment Tracking

<!-- W&B, MLflow, or other tracking configuration -->

- **Tool**: {e.g. Weights & Biases}
- **Project**: {W&B project name or URL}
- **Key dashboards**: {links if available}

---

## Reproducibility

<!-- How reproducibility is ensured -->

- **Seeds**: {How random seeds are managed, e.g. set in config, applied to torch/numpy/random}
- **Environment**: {How environment is pinned, e.g. requirements.txt, conda environment.yml}
- **Config versioning**: {e.g. each experiment has a unique config file, never overwrite old configs}

---

## Results Convention

<!-- How results are organized -->

- **Directory**: `results/{experiment-id}/`
- **Figures**: `results/figures/{experiment-id}_{plot-type}.pdf`
- **Tables**: `results/tables/{table-name}.tex`
- **Naming**: {experiment naming scheme, e.g. exp-NNN-short-description}

---

## Paper

<!-- Paper source and compilation -->

- **Source**: `paper/`
- **Main file**: `paper/main.tex`
- **Compile**: `{compile-command}`
- **Bibliography**: `paper/references.bib`

---

## Validation

<!-- Commands to run before committing -->

```bash
{validation-commands, e.g.
ruff check .
python -c "import torch; print(torch.cuda.is_available())"
python train.py --config configs/default.yaml --dry-run
}
```

---

## Key Files

<!-- Important files to know about -->

| File | Purpose |
|------|---------|
| `{path}` | {description} |

---

## On-Demand Context

<!-- Optional: Reference docs for deeper context. These are loaded into context when Claude needs them. -->

| Topic | File |
|-------|------|
| {topic} | `{path}` |

---

## Notes

<!-- Any special instructions, constraints, or gotchas -->

- {note}
