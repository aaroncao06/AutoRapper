# Claude Code Research Workflow

A workflow template for ML/DL researchers using [Claude Code](https://claude.ai/code). It gives Claude a structured set of commands that mirror the research lifecycle: planning, implementing, running experiments, analyzing results, and writing papers.

## TL;DR

You give Claude 11 slash commands that turn it into a research assistant with memory.

**Setup (once per project):**
1. Brainstorm your research idea with Claude
2. `/create-brief` — generate a research plan (hypotheses, methods, evaluation protocol)
3. `/create-rules` — teach Claude your codebase conventions
4. `/init-project` — set up environment, directories, experiment log

**Research loop (repeat):**
1. (in a new session) `/prime` → `/plan-task` — load context, then plan the next piece of work (in one session)
2. **Start another fresh session** — planning fills up context; a clean session gives better execution
3. `/execute` — implement the plan; for experiments: write code, dry-run, launch training, log it (does NOT wait for training to finish)
4. `/commit` — structured git commit with tags like `[exp]`, `[result]`, `[paper]`
5. *(wait for training to complete)*
6. `/analyze-results` — run evaluation, generate plots/tables, record whether hypothesis was supported
7. `/commit`

**Paper writing:**
1. `/write-paper <section>` — writes LaTeX directly from experiment data, pulling exact numbers and flagging claims that need more evidence
2. `/commit`

**The glue:** Everything connects through three shared files:
- `RESEARCH-BRIEF.md` — your research plan
- `EXPERIMENT-LOG.md` — your lab notebook
- `CLAUDE.md` — your project rules

Each command reads and writes to these, so context accumulates across sessions instead of being lost.

---

## Why This Exists

When you use Claude Code for research, you're essentially having a conversation with an AI agent that can read and write files, run commands, and search the web. But without structure, each session starts from scratch — Claude doesn't know your project conventions, your experiment history, or where you left off.

This template solves that by providing:
1. **Persistent project context** (`CLAUDE.md`) — so Claude always knows your codebase conventions
2. **A lab notebook** (`EXPERIMENT-LOG.md`) — so Claude knows what experiments have been run and their results
3. **Structured commands** — so complex multi-step workflows (planning an experiment, analyzing results, writing a paper section) happen consistently every time
4. **Session continuity** — so you can stop mid-work and resume later without losing context

## Setup

1. Copy this template into your research project (or fork it):
   ```bash
   cp -r claude-code-template/.agents your-project/.agents
   cp -r claude-code-template/.claude your-project/.claude
   cp claude-code-template/.env.example your-project/.env.example
   ```

2. Copy `.env.example` to `.env` and fill in your actual values (W&B API key, paths, etc.):
   ```bash
   cp .env.example .env
   ```
   The `.env.example` file exists so that Claude knows which environment variables your project needs — without ever seeing your actual secrets. This means when Claude runs `/execute` or `/init-project`, it understands the configuration requirements immediately and can move straight into implementation, database setup, and end-to-end testing without getting stuck or resorting to mock values. Commit `.env.example` to git; keep `.env` gitignored.

3. In your project, run Claude Code and use the setup commands:
   ```
   /create-brief    # Generate a research plan from conversation
   /create-rules    # Generate CLAUDE.md from your codebase
   /init-project    # Set up environment, directories, experiment log
   ```

## The Workflow

### One-Time Setup

Before you start experimenting, you establish the project foundation:

```
You ←→ Claude (brainstorm research idea, discuss methods)
         ↓
    /create-brief     →  RESEARCH-BRIEF.md (your research plan)
         ↓
    /create-rules     →  CLAUDE.md (project conventions for Claude)
         ↓
    /init-project     →  Environment, directories, EXPERIMENT-LOG.md
```

**`/create-brief`** generates a Research Brief — the research equivalent of a product spec. It captures your research question, hypotheses, planned experiments, evaluation protocol, and target venue. This is the document that keeps everything aligned.

**`/create-rules`** analyzes your codebase and generates a `CLAUDE.md` file — a set of rules that Claude reads at the start of every session. It describes your tech stack, coding conventions, project structure, and commands. This means Claude always knows how your project works.

**`/init-project`** sets up the practical stuff: Python environment, GPU verification, W&B, directory structure, and initializes the experiment log.

### The Task Loop

This is what you do repeatedly — plan a task, implement it, commit:

```
SESSION 1:
  /prime              Load project context into Claude's memory
     ↓
  /plan-task          Create a detailed implementation plan
     ↓
  /handoff            (optional) Save session state

SESSION 2 (fresh context):
  /execute            Implement the plan (and launch training if applicable)
     ↓
  /commit             Git commit with structured tags

SESSION 3 (after training completes):
  /analyze-results    Process completed experiment results
     ↓
  /commit             Commit the analysis
```

**Important: Use separate sessions for planning and execution.** After many messages, Claude's context window fills up — it starts to lose track of earlier details and repeat mistakes. Starting a fresh session for `/execute` gives Claude clean context and much better results. The plan file in `.agents/plans/` carries all the context forward, so nothing is lost. This is why `/plan-task` writes everything to a file rather than keeping it in conversation.

Here's what each step does:

**`/prime`** is always the first thing you run in a new session. It reads your research brief, CLAUDE.md, experiment log, and recent git history. After priming, Claude knows: what you're researching, what experiments have been run, what the current results are, and what state the paper is in.

**`/plan-task`** takes a task description and produces a detailed plan. The task can be anything — setting up an experiment, refactoring the data pipeline, adding a new model architecture, or running an ablation study. For experiments, it includes the hypothesis, config file, launch command, and evaluation protocol. Plans are saved to `.agents/plans/` so you can review them before executing.

**`/execute`** reads a plan file and implements it step by step. For experiments, it creates configs, writes code, runs a dry-run to verify everything works (with auto-retry if it hits common ML errors like OOM or shape mismatches), then launches training. Crucially, it does NOT wait for training to finish — it launches the run, records the W&B link in the experiment log, and returns control to you.

**`/commit`** makes a git commit with a structured tag reflecting the type of work: `[exp]` for experiment setup, `[result]` for analysis, `[paper]` for writing, etc. For experiment commits, it also runs a reproducibility checklist (config committed? seeds recorded? environment captured?).

**`/analyze-results`** is what you run after training finishes. Give it an experiment ID (e.g., `exp-003`) and it loads the checkpoints, runs evaluation, generates comparison tables and plots, computes statistical significance, and updates the experiment log with the results and a verdict (hypothesis supported/refuted/inconclusive).

### The Paper Loop

When you've accumulated enough evidence, you write:

```
/prime              Reload context
   ↓
/write-paper        Write or update a paper section
   ↓
/commit             Commit with [paper] tag
```

**`/write-paper`** takes a section name (`abstract`, `introduction`, `method`, `results`, `related-work`, `conclusion`, or `all`) and writes directly to your LaTeX files. It first builds an evidence map — which claims are supported by which experiments — then writes the section pulling exact numbers from results. It flags any claims that lack sufficient evidence, which might send you back to the experiment loop.

### Session Management

**`/handoff`** captures the current session state into a `HANDOFF.md` file: what's done, what's in progress, what experiments are running, key decisions made, dead ends tried. The next session can read this file and continue seamlessly.

**`/lit-review`** conducts a structured literature search on a topic, querying Semantic Scholar and organizing papers by theme. It extracts baseline numbers, generates BibTeX entries, and saves everything to `.agents/reference/`.

## File Structure

After setup, your project will have these Claude-specific files:

```
your-project/
├── CLAUDE.md                          # Project rules (Claude reads this every session)
├── RESEARCH-BRIEF.md                  # Research plan (hypotheses, methods, evaluation)
├── EXPERIMENT-LOG.md                  # Lab notebook (all experiments and results)
├── .agents/
│   ├── CLAUDE-template.md             # Template for generating CLAUDE.md
│   ├── EXPERIMENT-LOG-template.md     # Template for experiment log
│   ├── plans/                         # Task/experiment plans
│   └── reference/                     # Literature reviews, specs, docs
├── .claude/
│   ├── commands/                      # All the slash commands described above
│   ├── settings.json                  # Hooks (notifications, auto-formatting)
│   └── skills/                        # Custom skills (extensible)
└── .env.example                       # Environment variable template
```

## Commands Reference

| Command | When | What It Does |
|---------|------|-------------|
| `/create-brief` | Once | Generate research plan from conversation |
| `/create-rules` | Once | Generate CLAUDE.md from codebase analysis |
| `/init-project` | Once | Set up environment, directories, experiment log |
| `/lit-review <topic>` | As needed | Search papers, extract baselines, save structured review |
| `/prime` | Start of every session | Load project context into Claude's memory |
| `/plan-task <description>` | Before implementation | Create detailed plan with validation steps |
| `/execute <plan-file>` | After planning | Implement plan, launch training if applicable |
| `/analyze-results <exp-id>` | After training completes | Evaluate, compare, plot, update experiment log |
| `/write-paper <section>` | When evidence accumulates | Write LaTeX section from experiment results |
| `/commit` | After any work | Structured git commit with reproducibility checks |
| `/handoff` | End of session | Capture state for next session |

## How It All Connects

The key insight is that these commands don't work in isolation — they build on each other through shared files:

- **RESEARCH-BRIEF.md** is written by `/create-brief` and read by `/prime`, `/plan-task`, `/write-paper`, and `/lit-review`
- **EXPERIMENT-LOG.md** is initialized by `/init-project`, updated by `/execute` (adds RUNNING entries) and `/analyze-results` (adds results), and read by `/prime`, `/write-paper`, and `/handoff`
- **CLAUDE.md** is generated by `/create-rules` and read by Claude at the start of every session
- **Git commit history** is read by `/prime` to understand recent development — the structured commit tags make this meaningful
- **Plans in `.agents/plans/`** are written by `/plan-task` and read by `/execute`
- **Literature reviews in `.agents/reference/`** are written by `/lit-review` and read by `/plan-task` and `/write-paper`

This means the project accumulates context over time. Each session starts with `/prime`, which gives Claude full awareness of the project's current state — not just the code, but the research question, experiment history, and paper progress.

## Evolving the System

Every time you hit a recurring problem — Claude keeps making the same mistake, forgetting a convention, or producing code that doesn't match your style — that's a signal to improve the system, not just fix the code. Ask yourself:

- **Fix the rules?** Update `CLAUDE.md` with a new convention or constraint. Example: Claude keeps using `print()` instead of `logging.info()` → add a rule to CLAUDE.md.
- **Fix the context?** Add a reference doc to `.agents/reference/`. Example: Claude doesn't understand your custom data format → write a spec and add it to On-Demand Context.
- **Fix a command?** Update the command in `.claude/commands/`. Example: `/execute` doesn't check for a common failure mode → add it to the auto-debug table.

The goal is that your workflow gets smarter over time. Each fix prevents the same problem from recurring in every future session — not just this one.
