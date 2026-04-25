Create a new commit for all of our uncommitted changes.

Run `git status && git diff HEAD && git status --porcelain` to see what files are uncommitted.
Add the untracked and changed files.

Create an atomic commit message with an appropriate message.

Use one of these tags to reflect the type of work:
- `[exp]` — experiment setup, config, or training code
- `[result]` — analysis results, figures, tables
- `[paper]` — paper writing, LaTeX changes
- `[data]` — data pipeline, preprocessing, dataset changes
- `[config]` — configuration changes (non-experiment)
- `[fix]` — bug fixes
- `[refactor]` — code restructuring
- `[docs]` — documentation changes
- `[infra]` — tooling, environment, CI/CD

When committing experiment-related changes, include the experiment ID:
  e.g., `[exp] setup attention ablation (exp-003)`

When committing results, include the key metric:
  e.g., `[result] exp-002 analysis: 30.1 BLEU (+1.7 over baseline)`

When committing paper changes, note the section:
  e.g., `[paper] draft results section with exp-001 through exp-003`

## Reproducibility Check (for [exp] and [result] commits)

When committing with `[exp]` or `[result]` tags, verify these reproducibility items before committing. Warn (but don't block) if any are missing:

- [ ] **Config committed**: The experiment config file is staged (not just in working dir)
- [ ] **Seeds recorded**: Random seeds are specified in the config or EXPERIMENT-LOG.md entry
- [ ] **Environment captured**: `requirements.txt` or `environment.yml` is up to date with current packages
- [ ] **Experiment logged**: EXPERIMENT-LOG.md has an entry for this experiment

If any are missing, print a warning like:
```
WARNING: Reproducibility gap detected:
- requirements.txt may be outdated (last modified 2 weeks ago)
- No random seed found in configs/exp-005.yaml
Consider updating before committing. Proceeding anyway.
```
