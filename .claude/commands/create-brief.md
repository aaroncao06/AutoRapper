---
description: Create a Research Brief from conversation
argument-hint: [output-filename]
---

# Create Research Brief

## Overview

Generate a comprehensive Research Brief based on the current conversation context — the research question, hypotheses, methodology, and experiment design discussed so far. This replaces the traditional PRD for academic ML/DL research.

## Output File

Write the Research Brief to: `$ARGUMENTS` (default: `RESEARCH-BRIEF.md`)

## Research Brief Structure

Create a well-structured brief with the following sections. Adapt depth and detail based on available information:

### Required Sections

**1. Research Summary**
- Concise overview (2-3 paragraphs)
- What gap in the literature does this address?
- What is the proposed approach?
- What is the expected contribution?

**2. Research Question**
- Formal statement of the central research question
- Why this question matters

**3. Paper Scope / Claims**
- **In Scope:** Claims this paper will make (use checkboxes)
  - [ ] Claim 1: {specific, testable claim}
  - [ ] Claim 2: {specific, testable claim}
- **Future Work / Out of Scope:** Deferred to follow-up work (use checkboxes)
  - [ ] {extension 1}
  - [ ] {extension 2}

**4. Hypotheses**
- Numbered hypotheses, each with:
  - **H1**: {precise statement}
  - **Expected outcome**: {what we expect to observe}
  - **Experiment**: {which experiment tests this}
  - **Reasoning**: {why we expect this outcome}

**5. Method Overview**
- High-level approach: model architecture, training procedure, datasets
- Key innovations or differences from prior work
- Diagram or pipeline description if helpful

**6. Experiment Groups**
- Organize experiments by which claim they support:
  - **Claim 1 experiments:** baseline reproduction, main comparison, ablation A
  - **Claim 2 experiments:** analysis X, ablation B
- Include estimated compute per group

**7. Compute & Software Stack**
- ML framework and version (e.g., PyTorch 2.x)
- Key libraries with versions
- Experiment tracking tool (e.g., Weights & Biases)
- Compute resources (GPU type, count, estimated hours)
- Environment management (conda, pip, Docker)

**8. Reproducibility Requirements**
- Seed management strategy
- Environment pinning approach
- Config management approach (recommend: standalone YAML files in `configs/`)
- Checkpoint policy (what to save, when, how long to keep)
- Data versioning (if applicable)

**9. Evaluation Protocol**
- Primary and secondary metrics
- Datasets and splits (train/val/test)
- Baselines to compare against (with known numbers if available)
- Statistical tests for significance (e.g., paired t-test across seeds)
- Significance thresholds

**10. Experiment Phases**
- Phase 1: Baseline reproduction (verify setup, reproduce known results)
- Phase 2: Core experiments (test main hypotheses)
- Phase 3: Ablations and analysis (understand what matters)
- Each phase includes: Goal, experiments, validation criteria

**11. Related Work Summary**
- 5-10 key papers to compare against
- Their reported results on relevant benchmarks
- Identified gaps that this work addresses
- How this work differs from each

**12. Risks & Mitigations**
- 3-5 key risks with specific mitigation strategies
- Examples: compute cost overrun, negative results contingency, baseline unavailability, reviewer objections

**13. Future Work**
- Natural extensions for the paper's conclusion
- Longer-term research directions

**14. Deliverables**
- Target venue and format (conference, journal, workshop)
- Supplementary materials plan (code, appendix, data)
- Code release plan (repository, license)
- Timeline estimates

## Instructions

### 1. Extract Requirements
- Review the entire conversation history
- Identify the research question, hypotheses, and methodology discussed
- Note technical constraints (compute, data, time)
- Capture evaluation criteria and baselines

### 2. Synthesize Information
- Organize into the sections above
- Fill in reasonable assumptions where details are missing
- Maintain consistency across sections (hypotheses match experiments match claims)
- Ensure scientific rigor (controls, baselines, statistical testing)

### 3. Write the Brief
- Use clear, precise scientific language
- Include concrete numbers where possible (metric targets, compute estimates)
- Use markdown formatting (headings, lists, tables, checkboxes)
- Be specific about datasets, metrics, and baselines

### 4. Quality Checks
- All required sections present
- Hypotheses are precise and testable
- Every claim has at least one supporting experiment planned
- Evaluation protocol is rigorous (baselines, significance tests)
- Experiment phases are actionable and ordered by dependency
- Compute estimates are realistic
- Consistent terminology throughout

## Style Guidelines

- **Tone:** Scientific, precise, action-oriented
- **Format:** Use markdown extensively (headings, lists, tables, checkboxes)
- **Specificity:** Include concrete numbers, metric names, dataset names
- **Length:** Comprehensive but scannable

## Output Confirmation

After creating the Research Brief:
1. Confirm the file path where it was written
2. Provide a brief summary of the research plan
3. Highlight any assumptions made due to missing information
4. Suggest next steps (e.g., `/create-rules`, literature deep-dive, data acquisition)

## Notes

- If critical information is missing (research question, datasets, baselines), ask clarifying questions before generating
- Adapt section depth based on available details
- For theory-heavy work, emphasize hypotheses and analysis; for systems work, emphasize experiments and benchmarks
- This command contains the complete brief template — no external references needed
