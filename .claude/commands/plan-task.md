---
description: "Create comprehensive task plan with deep codebase analysis and research"
---

# Plan a new task

## Task: $ARGUMENTS

## Mission

Transform a task request into a **comprehensive implementation plan** through systematic codebase analysis, external research, and strategic planning. The task may be an experiment, a code change, data pipeline work, analysis, or any other research-related work.

**Core Principle**: We do NOT write code in this phase. Our goal is to create a context-rich plan that enables one-pass implementation success for AI agents.

**Key Philosophy**: Context is King. The plan must contain ALL information needed for implementation — patterns, mandatory reading, documentation, validation commands — so the execution agent succeeds on the first attempt.

## Planning Process

### Phase 1: Task Understanding

**Deep Task Analysis:**

- Extract the core problem being solved
- Determine task type:
  - **Experiment**: New training run testing a hypothesis (needs config, launch command, eval plan)
  - **Code Change**: Modifying existing code (model architecture, data pipeline, training loop)
  - **Analysis**: Processing results, generating figures, statistical tests
  - **Infrastructure**: Environment setup, tooling, scripts
  - **Paper**: Writing or editing paper sections
- Assess complexity: Low/Medium/High
- Map affected systems and components
- Identify which paper claim or hypothesis this supports (if applicable)

**For Experiment Tasks, Also Define:**

```
Hypothesis: H<N>: {precise statement}
Expected Outcome: {what we expect to observe}
Experiment Type: [Baseline Reproduction / Core Experiment / Ablation / Analysis]
```

### Phase 2: Codebase Intelligence Gathering

**Use specialized agents and parallel analysis:**

**1. Project Structure Analysis**

- Detect primary language(s), frameworks, and runtime versions
- Map directory structure and architectural patterns
- Locate configuration files (pyproject.toml, configs/, etc.)
- Find environment setup and training entry points

**2. Pattern Recognition** (Use specialized subagents when beneficial)

- Search for similar implementations in codebase
- Identify coding conventions:
  - Config management pattern (YAML, Hydra, argparse)
  - Model definition pattern (nn.Module subclasses, factories)
  - Training loop pattern (custom, Lightning, HuggingFace Trainer)
  - Metric logging pattern (W&B, TensorBoard, CSV)
  - Checkpoint pattern (save/load conventions)
- Extract common patterns for the task's domain
- Document anti-patterns to avoid
- Check CLAUDE.md for project-specific rules and conventions

**3. Dependency Analysis**

- Catalog external libraries relevant to task
- Understand how libraries are integrated (check imports, configs)
- Find relevant documentation in docs/, .agents/reference/ if available
- Note library versions and compatibility requirements

**4. Evaluation Patterns** (for experiment tasks)

- Identify evaluation framework and metrics computation
- Find similar evaluation examples for reference
- Understand how results are stored and compared
- Note baseline numbers and where they come from

**5. Integration Points**

- Identify existing files that need updates
- Determine new files that need creation and their locations
- Map config file patterns (how to add a new experiment config)
- Understand data pipeline if relevant

**Clarify Ambiguities:**

- If requirements are unclear at this point, ask the user to clarify before you continue
- Get specific implementation preferences (libraries, approaches, patterns)
- Resolve design decisions before proceeding

### Phase 3: External Research & Documentation

**Use specialized subagents when beneficial for external research:**

**Documentation Gathering:**

- Research latest library versions and best practices
- Find official documentation with specific section anchors
- Locate implementation examples and tutorials
- Identify common gotchas and known issues

**For Experiment Tasks — Literature & Baselines:**

- Search for papers with comparable methods
- Note their reported metrics on relevant benchmarks
- Identify their experimental setups for fair comparison
- Gather known baseline numbers from papers, leaderboards, or prior experiments in EXPERIMENT-LOG.md

**Compile Research References:**

```markdown
## Relevant Documentation

- [Library Official Docs](https://example.com/docs#section)
  - Specific section: {what's relevant}
  - Why: Needed for X functionality
- [Paper/Baseline Reference](https://arxiv.org/abs/XXXX.XXXXX)
  - Reported metrics: {numbers}
  - Why: Baseline comparison
```

### Phase 4: Deep Strategic Thinking

**Think Harder About:**

- How does this task fit into the existing codebase and research plan?
- What are the critical dependencies and order of operations?
- What could go wrong? (Edge cases, training instability, data issues, unfair comparisons)
- How will results be validated?
- What performance implications exist? (Memory, compute time)
- How reproducible is this? (Seeds, configs, environment)

**For Experiment Tasks, Additionally:**

- What are the independent and dependent variables?
- What is the compute budget?
- How many seeds are needed for statistical significance?
- What baselines ensure a fair comparison?

**Design Decisions:**

- Choose between alternative approaches with clear rationale
- Plan for reproducibility
- Consider how this fits into the broader paper narrative

### Phase 5: Plan Structure Generation

**Create comprehensive plan with the following structure:**

What's below here is a template for you to fill for the execution agent:

```markdown
# Task: <task-name>

The following plan should be complete, but it's important that you validate documentation and codebase patterns and task sanity before you start implementing.

Pay special attention to naming of existing utils, types, and models. Import from the right files etc.

## Task Description

<Detailed description of the task, its purpose, and how it fits into the research>

## Task Metadata

**Task Type**: [Experiment / Code Change / Analysis / Infrastructure / Paper]
**Estimated Complexity**: [Low/Medium/High]
**Primary Systems Affected**: [List of main components/files]
**Dependencies**: [External libraries or data required]
**Supports Claim/Hypothesis**: [Which paper claim or hypothesis, if any]

---

## FOR EXPERIMENT TASKS ONLY — Experiment Specification

### Hypothesis

<precise statement: "We hypothesize that [intervention] will [improve/change] [metric] because [reasoning]">

### Experimental Setup

**Model Configuration:**
<model architecture details, key hyperparameters>

**Dataset:**
<dataset name, splits, preprocessing>

**Training Configuration:**
<batch size, learning rate, epochs, optimizer, scheduler, etc.>

### Config File

<Path to config file, or inline YAML if creating a new one>

```yaml
# configs/exp-NNN-short-name.yaml
model:
  ...
data:
  ...
training:
  ...
```

### Evaluation Protocol

**Metrics:** <primary and secondary>
**Baselines:** <what to compare against, where their results are stored>
**Statistical Tests:** <if applicable, e.g. paired t-test across N seeds>

### Launch Command

```bash
<exact command to start training>
```

**Expected Duration:** <estimated hours/days>
**Monitoring:** <W&B project/run link pattern>
**Checkpoint Location:** <path>

### Expected Outcomes

<what confirms/refutes the hypothesis>

### Compute Budget

<GPU hours, cost estimate if relevant>

---

## CONTEXT REFERENCES

### Relevant Codebase Files IMPORTANT: YOU MUST READ THESE FILES BEFORE IMPLEMENTING!

<List files with line numbers and relevance>

- `path/to/file.py` (lines 15-45) - Why: Contains pattern for X that we'll mirror
- `path/to/model.py` (lines 100-120) - Why: Model structure to follow
- `path/to/config.yaml` - Why: Config format example

### New Files to Create

- `configs/exp-NNN-short-name.yaml` - Experiment configuration
- `path/to/new_module.py` - Module implementation

### Relevant Documentation YOU SHOULD READ THESE BEFORE IMPLEMENTING!

- [Documentation Link 1](https://example.com/doc1#section)
  - Specific section: {what}
  - Why: Required for implementing X

### Patterns to Follow

<Specific patterns extracted from codebase — include actual code examples from the project>

**Config Pattern:** (for example)

**Model Definition Pattern:** (for example)

**Logging Pattern:** (for example)

---

## IMPLEMENTATION PLAN

### Phase 1: Setup

<Foundational work before main implementation>

**Tasks:**

- Create/modify config files
- Set up any new data loading
- Create directory structure if needed

### Phase 2: Core Implementation

<Main implementation work>

**Tasks:**

- Implement model changes or new code
- Update training/evaluation scripts
- Add metric logging

### Phase 3: Validation

<Verify everything works before launch>

**Tasks:**

- Dry-run training (1 step) to verify setup
- Check data loading, loss computation, metric logging
- Verify checkpoint save/load

### Phase 4: Launch (for experiments) / Testing (for code changes)

**For experiments:**
- Launch training run
- Verify training started (check logs or W&B)
- Update EXPERIMENT-LOG.md

**For code changes:**
- Run existing tests
- Add new tests if needed
- Verify no regressions

---

## STEP-BY-STEP TASKS

IMPORTANT: Execute every task in order, top to bottom. Each task is atomic and independently testable.

### Task Format Guidelines

Use information-dense keywords for clarity:

- **CREATE**: New files or components
- **UPDATE**: Modify existing files
- **ADD**: Insert new functionality into existing code
- **REMOVE**: Delete deprecated code
- **REFACTOR**: Restructure without changing behavior
- **MIRROR**: Copy pattern from elsewhere in codebase
- **CONFIG**: Create or modify experiment configuration
- **LAUNCH**: Start a training run (async — do not wait)
- **LOG**: Update EXPERIMENT-LOG.md

### {ACTION} {target_file}

- **IMPLEMENT**: {Specific implementation detail}
- **PATTERN**: {Reference to existing pattern — file:line}
- **IMPORTS**: {Required imports and dependencies}
- **GOTCHA**: {Known issues or constraints to avoid}
- **VALIDATE**: `{executable validation command}`

<Continue with all tasks in dependency order...>

---

## VALIDATION COMMANDS

<Define validation commands based on project's tools discovered in Phase 2>

### Level 1: Syntax & Style

<Project-specific linting commands>

### Level 2: Dry Run

<1-step training or execution to verify setup>

### Level 3: Existing Tests

<Run existing test suite to check for regressions>

### Level 4: Manual Validation

<Task-specific checks — verify outputs, check W&B, inspect configs>

---

## ACCEPTANCE CRITERIA

<List specific, measurable criteria that must be met for completion>

- [ ] Task implements all specified functionality
- [ ] All validation commands pass with zero errors
- [ ] Code follows project conventions and patterns
- [ ] No regressions in existing functionality
- [ ] Config files are complete and valid
- [ ] For experiments: training launched and logging to W&B
- [ ] For experiments: EXPERIMENT-LOG.md updated
- [ ] For code changes: tests pass

---

## COMPLETION CHECKLIST

- [ ] All tasks completed in order
- [ ] Each task validation passed immediately
- [ ] All validation commands executed successfully
- [ ] Manual verification confirms task works
- [ ] Acceptance criteria all met
- [ ] Ready for `/commit`

---

## NOTES

<Additional context, design decisions, trade-offs>
```

## Output Format

**Filename**: `.agents/plans/{kebab-case-descriptive-name}.md`

- Replace `{kebab-case-descriptive-name}` with short, descriptive task name
- Examples: `exp-003-attention-ablation.md`, `add-mixed-precision-training.md`, `refactor-data-pipeline.md`, `baseline-reproduction.md`

**Directory**: Create `.agents/plans/` if it doesn't exist

## Quality Criteria

### Context Completeness

- [ ] All necessary patterns identified and documented
- [ ] External library usage documented with links
- [ ] Integration points clearly mapped
- [ ] Gotchas and anti-patterns captured
- [ ] Every task has executable validation command

### Implementation Ready

- [ ] Another developer/agent could execute without additional context
- [ ] Tasks ordered by dependency (can execute top-to-bottom)
- [ ] Each task is atomic and independently testable
- [ ] Pattern references include specific file:line numbers

### For Experiments

- [ ] Hypothesis is precise and testable
- [ ] Config file is complete and valid
- [ ] Launch command is exact and executable
- [ ] Evaluation protocol specifies metrics, baselines, and statistical tests
- [ ] Compute budget is estimated

### Information Density

- [ ] No generic references (all specific and actionable)
- [ ] URLs include section anchors when applicable
- [ ] Task descriptions use codebase keywords
- [ ] Validation commands are non-interactive and executable

## Success Metrics

**One-Pass Implementation**: Execution agent can complete task without additional research or clarification

**Validation Complete**: Every task has at least one working validation command

**Context Rich**: The plan passes "No Prior Knowledge Test" — someone unfamiliar with codebase can implement using only plan content

**Confidence Score**: #/10 that execution will succeed on first attempt

## Report

After creating the plan, provide:

- Summary of task and approach
- Full path to created plan file
- Complexity assessment
- Key implementation risks or considerations
- Estimated confidence score for one-pass success
