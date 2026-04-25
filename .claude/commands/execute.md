---
description: Execute a task plan
argument-hint: [path-to-plan]
---

# Execute: Implement from Plan

## Plan to Execute

Read plan file: `$ARGUMENTS`

## Execution Instructions

### 1. Read and Understand

- Read the ENTIRE plan carefully
- Understand all tasks and their dependencies
- Note the validation commands to run
- Identify if this is an experiment task (has Experiment Specification section) or a code/infrastructure task
- Review the task type and acceptance criteria

### 2. Execute Tasks in Order

For EACH task in "Step by Step Tasks":

#### a. Navigate to the task
- Identify the file and action required
- Read existing related files if modifying

#### b. Implement the task
- Follow the detailed specifications exactly
- Maintain consistency with existing code patterns
- Include proper type hints and documentation
- Add structured logging where appropriate

#### c. Verify as you go
- After each file change, check syntax
- Ensure imports are correct
- Verify types are properly defined

### 3. Validate Implementation

After completing implementation tasks:

#### For all tasks:
- Run all validation commands from the plan in order
- If any command fails: fix the issue, re-run, continue only when it passes

#### For experiment tasks:
- Run dry-run validation (1 training step) to verify:
  - Model builds correctly
  - Data loads without errors
  - Loss computes and is finite
  - Metrics log to W&B/tracking tool
  - Checkpoint saves correctly

#### Auto-Debug on Dry-Run Failure (up to 3 retries)

If the dry-run fails, diagnose the error and attempt an automatic fix before retrying:

| Error Type | Diagnosis | Auto-Fix |
|-----------|-----------|----------|
| **OOM (OutOfMemoryError)** | Check batch size, model size, GPU memory | Reduce batch size by half, suggest gradient accumulation to compensate |
| **ImportError / ModuleNotFoundError** | Check package name and environment | Run `pip install {package}`, verify version compatibility |
| **Shape/dimension mismatch** | Trace tensor dimensions through the error stack | Fix the mismatched dimension in model code or config |
| **CUDA error (device mismatch)** | Check `.to(device)` calls and CUDA_VISIBLE_DEVICES | Add missing device transfers, verify GPU availability |
| **FileNotFoundError (data)** | Check data paths in config vs. actual filesystem | Fix path in config, suggest data download if missing |
| **NaN/Inf loss** | Check learning rate, initialization, data preprocessing | Reduce learning rate, add gradient clipping, check data for NaN |

**Retry protocol:**
1. Diagnose the specific error from the stack trace
2. Apply the targeted fix
3. Re-run the dry-run
4. If it fails again with a DIFFERENT error, repeat (up to 3 total attempts)
5. If it fails 3 times or with the SAME error twice, stop and report the issue to the user

#### For code changes:
- Run existing test suite to check for regressions
- Create and run any new tests specified in the plan

### 4. Launch (for experiment tasks)

If the plan includes a LAUNCH task:

1. Execute the exact launch command from the plan
2. Verify training has started:
   - Check first few log lines for successful initialization
   - Verify W&B run is active (if applicable)
   - Confirm checkpoint directory is being written to
3. Record the monitoring information:
   - W&B run URL or run ID
   - Log file path
   - Expected completion time

**IMPORTANT: Do NOT wait for training to complete.** Training runs asynchronously.

### 5. Update Experiment Log (for experiment tasks)

If this is an experiment task, append a new entry to `EXPERIMENT-LOG.md`:

```markdown
### exp-NNN: {short name}

- **Hypothesis**: {from plan}
- **Config**: `configs/{config-file}.yaml`
- **Status**: RUNNING
- **Started**: {current date and time}
- **Completed**: —
- **Seeds**: {from plan}
- **W&B Run**: {run URL or ID}
- **Checkpoint**: `checkpoints/{path}`

**Results** (filled by /analyze-results):
- {Primary metric}: —
- **vs Baseline**: —
- **Assessment**: —

**Notes**: {any observations from setup/launch}
```

Also update the summary table at the top of EXPERIMENT-LOG.md.

### 6. Final Verification

Before completing:

- All tasks from plan completed
- All validation commands pass
- Code follows project conventions
- For experiments: training launched and logging correctly
- For experiments: EXPERIMENT-LOG.md updated
- For code changes: tests pass, no regressions

## Output Report

Provide summary:

### Completed Tasks
- List of all tasks completed
- Files created (with paths)
- Files modified (with paths)

### Validation Results
```bash
# Output from each validation command
```

### For Experiment Tasks
- **Experiment ID**: exp-NNN
- **W&B Run**: {URL or ID}
- **Status**: RUNNING
- **Expected Completion**: {estimate}
- **Next Step**: Run `/analyze-results exp-NNN` when training completes

### For Code Changes
- Test results
- Any regressions found and fixed

### Ready for Commit
- Confirm all changes are complete
- Confirm all validations pass
- Ready for `/commit` command

## Notes

- If you encounter issues not addressed in the plan, document them
- If you need to deviate from the plan, explain why
- For experiments: do NOT wait for training to finish — launch and move on
- Don't skip validation steps
- If dry-run fails, fix the issue before launching full training
