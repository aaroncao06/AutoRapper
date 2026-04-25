# Experiment Log

Canonical record of all experiments. Updated by `/execute` (adds RUNNING entry) and `/analyze-results` (updates with results).

## Summary

| ID | Hypothesis | Status | Primary Metric | Date |
|----|-----------|--------|---------------|------|
| exp-001 | {hypothesis} | {RUNNING/COMPLETED/FAILED} | {metric value} | {YYYY-MM-DD} |

---

## Detailed Entries

### exp-001: {short name}

- **Hypothesis**: {precise statement}
- **Config**: `configs/{config-file}.yaml`
- **Status**: {RUNNING / COMPLETED / FAILED}
- **Started**: {YYYY-MM-DD HH:MM}
- **Completed**: {YYYY-MM-DD HH:MM or "—"}
- **Seeds**: {list of seeds}
- **W&B Run**: {link or run ID}
- **Checkpoint**: `checkpoints/{path}`

**Results** (filled by /analyze-results):
- {Primary metric}: {value}
- {Secondary metric}: {value}
- **vs Baseline**: {+/- delta}
- **Assessment**: {supported / refuted / inconclusive}

**Notes**: {anything notable — training instability, unexpected behavior, etc.}

---

<!-- Copy the template above for each new experiment -->
