---
description: Prime agent with research project understanding
---

# Prime: Load Project Context

## Objective

Build comprehensive understanding of the research project by analyzing structure, documentation, experiment state, and key files.

## Process

### 1. Analyze Project Structure

List all tracked files:
!`git ls-files`

Show directory structure:
On Linux/Mac, run: `tree -L 3 -I 'node_modules|__pycache__|.git|dist|build|checkpoints|wandb|.venv'`

### 2. Read Core Documentation

- Read `RESEARCH-BRIEF.md` (research plan, hypotheses, claims)
- Read `CLAUDE.md` (global rules, conventions, commands)
- Read README files at project root
- Read `EXPERIMENT-LOG.md` (experiment history and results)
- Scan `configs/` directory listing (understand experiment landscape)
- Scan `results/` directory listing (understand what outputs exist)

### 3. Identify Key Files

Based on the structure, identify and read:
- Main training script (train.py, run.py, main.py, etc.)
- Model definition files
- Default/base config file
- Evaluation script(s)
- Key data loading/preprocessing files

### 4. Understand Current State

Check recent activity:
!`git log -10 --oneline`

Check current branch and status:
!`git status`

Check experiment state:
- How many experiments in EXPERIMENT-LOG.md?
- Any currently RUNNING?
- Latest COMPLETED results?
- Any FAILED that need attention?

## Output Report

Provide a concise summary covering:

### Research Question
- What is being investigated
- Key hypotheses

### Experiment State
- Completed experiments and their outcomes
- Currently running experiments
- Planned but not yet started
- Any failures needing attention

### Key Results
- Best metrics achieved so far
- Baseline comparisons
- Which hypotheses are supported/refuted/untested

### Research Stack
- ML framework and version
- Experiment tracking tool
- Compute setup
- Paper tooling

### Paper Status
- Which sections are drafted (check `paper/` directory)
- Which claims have sufficient evidence
- Gaps that need more experiments

### Current State
- Active branch
- Recent changes or development focus
- Any immediate observations or concerns

**Make this summary easy to scan — use bullet points and clear headers.**
