---
description: Create global rules (CLAUDE.md) from codebase analysis
---

# Create Global Rules

Generate a CLAUDE.md file by analyzing the codebase and extracting patterns.

---

## Objective

Create project-specific global rules that give Claude context about:
- What this research project investigates
- Technologies and frameworks used
- How code and experiments are organized
- Patterns and conventions to follow
- How to train, evaluate, and validate

---

## Phase 1: DISCOVER

### Identify Project Type

First, determine what kind of research project this is:

| Type | Indicators |
|------|------------|
| ML Training Pipeline | `train.py`, `configs/`, `checkpoints/`, model definitions |
| Experiment Framework | experiment configs, sweep configs, hyperparameter YAML |
| Paper Repository | `paper/`, `.tex` files, `figures/`, `results/` |
| Data Pipeline | `data/`, preprocessing scripts, dataset loaders |
| Library/Package | `main`/`exports` in setup.py/pyproject.toml, publishable |
| Mixed | Combination of the above (common for research repos) |

### Analyze Configuration

Look at root configuration files:

```
pyproject.toml / setup.py   -> dependencies, project metadata
requirements.txt             -> pinned dependencies
configs/                     -> experiment configurations (YAML/JSON)
Makefile / justfile          -> common commands (train, eval, plot)
wandb/ or mlflow/            -> experiment tracking setup
environment.yml              -> conda environment
Dockerfile                   -> reproducible environment
.env / .env.example          -> environment variables
```

### Map Directory Structure

Explore the codebase to understand organization:
- Where does model code live?
- Where are configs and experiment definitions?
- Where are results, figures, and tables stored?
- Where is the paper source?
- Where are scripts (training, eval, plotting)?

---

## Phase 2: ANALYZE

### Extract Research Stack

From config files and imports, identify:
- **ML Framework**: PyTorch, JAX, TensorFlow (and version)
- **Experiment Tracking**: W&B, MLflow, TensorBoard, or manual
- **Data Loading**: HuggingFace datasets, custom DataLoader, torchvision, etc.
- **Compute Setup**: GPU type, distributed training framework (DDP, FSDP, Lightning, Accelerate)
- **Visualization**: matplotlib, seaborn, plotly
- **Paper Tools**: LaTeX distribution, BibTeX, compilation command

### Identify Patterns

Study existing code for:
- **Config Management**: How are experiment configs structured? (YAML files, Hydra, argparse, dataclasses)
- **Model Definition**: How are models defined? (nn.Module subclasses, registration pattern, factory)
- **Training Loop**: Custom loop, PyTorch Lightning Trainer, HuggingFace Trainer?
- **Metric Logging**: How are metrics recorded? (wandb.log, TensorBoard writer, CSV)
- **Checkpoint Pattern**: How/where are models saved and loaded?
- **Results Storage**: Where do outputs, metrics, and figures land?

### Find Key Files

Identify files that are important to understand:
- Training entry point (train.py, run.py, main.py)
- Model definitions
- Default/base config
- Evaluation script
- Data loading/preprocessing
- Plotting/analysis scripts
- Paper source files

---

## Phase 3: GENERATE

### Create CLAUDE.md

Use the template at `.agents/CLAUDE-template.md` as a starting point.

**Output path**: `CLAUDE.md` (project root)

**Adapt to the project:**
- Remove sections that don't apply
- Add sections specific to this project
- Keep it concise — focus on what's useful for Claude

**Key sections to always include:**

1. **Project Overview** - What is this research about?
2. **Research Stack** - What technologies are used?
3. **Commands** - How to train, evaluate, plot, compile paper?
4. **Structure** - How is the code organized?
5. **Code Patterns** - Config management, model definition, training loop conventions
6. **Key Files** - What files are important to know?

**Important sections for research projects:**

- Experiment Tracking (W&B project, dashboards)
- Reproducibility (seeds, env pinning, config versioning)
- Results Convention (directory structure, naming scheme)
- Paper (source location, compile command)
- On-demand context references (reference docs in `.agents/reference/`)

---

## Phase 4: OUTPUT

```markdown
## Global Rules Created

**File**: `CLAUDE.md`

### Project Type

{Detected project type}

### Research Stack Summary

{Key technologies detected}

### Structure

{Brief structure overview}

### Next Steps

1. Review the generated `CLAUDE.md`
2. Add any project-specific notes
3. Remove any sections that don't apply
4. Create reference docs in `.agents/reference/` for specs, related work notes, etc.
```

---

## Tips

- Keep CLAUDE.md focused and scannable
- Don't duplicate information that's in other docs (link instead with On-Demand Context)
- Focus on patterns and conventions, not exhaustive documentation
- Update it as the project evolves and new patterns emerge
- For brownfield projects: detect existing structure and adapt rather than prescribing
