---
description: Initialize ML research project environment
---

# Initialize Project

Set up the research project environment locally.

## 1. Create Environment File
```bash
cp .env.example .env
```
Creates your local environment configuration from the example template. Fill in your W&B API key and adjust paths if needed.

## 2. Set Up Python Environment

Create and activate a virtual environment:
```bash
# Using conda (preferred for ML):
conda create -n {project-name} python=3.11 -y
conda activate {project-name}

# Or using venv:
python -m venv .venv
source .venv/bin/activate
```

Install dependencies:
```bash
pip install -r requirements.txt
# Or: pip install -e .
# Or: uv sync
```

## 3. Verify GPU Access
```bash
python -c "import torch; print(f'CUDA available: {torch.cuda.is_available()}'); print(f'GPU: {torch.cuda.get_device_name(0) if torch.cuda.is_available() else \"None\"}')"
```

## 4. Set Up Experiment Tracking
```bash
wandb login
wandb init
```
Verify W&B is configured and can log to the correct project.

## 5. Set Up Data

Download or symlink datasets:
```bash
# Project-specific data setup commands go here
# e.g., python scripts/download_data.py
# e.g., ln -s /shared/datasets/imagenet data/imagenet
```
Verify data loading works:
```bash
python -c "from data import get_dataloader; dl = get_dataloader('train', batch_size=2); print(f'Data loaded: {len(dl)} batches')"
```

## 6. Verify Paper Tooling
```bash
# Check LaTeX compilation
which latexmk && echo "latexmk found" || echo "latexmk not found — install texlive"
```
If `paper/` exists with `.tex` files, verify compilation:
```bash
cd paper && latexmk -pdf main.tex && cd ..
```

## 7. Create Directory Structure

Create standard directories if they don't exist:
```bash
mkdir -p configs results/figures results/tables scripts
# Gitignored directories:
mkdir -p checkpoints
echo "checkpoints/" >> .gitignore
```

## 8. Validate Setup

Run a 1-step training dry run to verify everything works end-to-end:
```bash
python train.py --config configs/default.yaml --dry-run
# Or whatever the project's training command is
```
This should verify: model builds, data loads, loss computes, metrics log, checkpoint saves.

## 9. Initialize Experiment Log

If `EXPERIMENT-LOG.md` doesn't exist, create it from the template:
```bash
cp .agents/EXPERIMENT-LOG-template.md EXPERIMENT-LOG.md
```

## Validate Everything

Quick checklist:
```bash
# Python environment works
python --version

# GPU accessible
python -c "import torch; assert torch.cuda.is_available(), 'No GPU'"

# W&B configured
python -c "import wandb; print(wandb.api.api_key[:8] + '...')"

# Data loads
python -c "from data import get_dataloader; print('OK')"
```

## Cleanup

To deactivate environment:
```bash
conda deactivate
# Or: deactivate
```
