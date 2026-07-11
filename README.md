# Aydin Capability Library

The professional knowledge layer of the **Professor Aydin** autonomous research
scientist (hermes-ai-professor), extracted as a standalone library:
**30 capability files** + the distilled **hard-rule sets** the
pipeline injects into each LLM agent.

**Version: hermes-ai-professor commit 0936a45 (2026-07-09 08:28 -0700; captures the 2026-07-08 post-professor-review-overhaul state, BEFORE the 07-09/10 dispatcher routing fixes and the r2 fix batch).**

Every rule marked "a live external review / a live run" was distilled from a
real failure of a real generated paper — this library is failure-driven, not
aspirational.

## Layout

- `stages/` — the 30 capability files, grouped by the pipeline stage that owns
  them; every stage folder is flat
- `rules/` — hard-rule sets exported verbatim from `capability_dispatcher.py`:
  one file per pipeline agent (`agent_*.md`) plus the universal sets injected
  into every prose writer
- `mapping.json` — machine-readable stage → capabilities and agent → files/rules
  maps + injection budgets

Each capability is stored once under its primary stage. Agents in later stages
can reuse an earlier-stage capability through `mapping.json`; the file is not
duplicated.

```text
stages/
├── stage-1-research/
├── stage-1.5-evidence-execution/
├── stage-2-write/
├── stage-2.5-pre-review-integrity/
├── stage-3-review/
└── stage-4-revise/
```

Stage 4.5 and Stage 5 remain explicit in `mapping.json`, but this snapshot has no
capability files owned exclusively by those stages.

## How the pipeline consumes this

1. **Hard rules first** (highest priority, never truncated).
2. **Capability files second**, budgeted: 3000 chars/file,
   12500 total (~4 files) for inline prompts; real SUBAGENTS read
   the full files from disk.
3. Matching is by substring token on the agent name (see `mapping.json`).

## Stage coverage

| stage | primary capability files | agents |
|---|---:|---|
| Stage 1 · Research | 4 | `literature_synthesizer` |
| Stage 1.5 · Evidence execution | 6 | `experiment_engineer` |
| Stage 2 · Write | 9 | `structure_architect`, `draft_writer`, `abstract_writer` |
| Stage 2.5 · Pre-review integrity | 2 | — |
| Stage 3 · Review | 7 | review panel and editorial agents |
| Stage 4 · Revise | 2 | `revision_writer` |
| Stage 4.5 · Final integrity | 0 | `final_paper_evaluator` |
| Stage 5 · Finalize | 0 | — |

## Agent coverage

| stage | agent token | hard rules | capability files |
|---|---|---:|---:|
| Stage 1 | `literature_synthesizer` | 3 | 4 |
| Stage 1.5 | `experiment_engineer` | 15 | 6 |
| Stage 2 | `abstract_writer` | 7 | 4 |
| Stage 2 | `draft_writer` | 19 | 5 |
| Stage 2 | `structure_architect` | 11 | 4 |
| Stage 3 | `devils_advocate` | 1 | 3 |
| Stage 3 | `domain_reviewer` | 1 | 2 |
| Stage 3 | `editorial` | 1 | 3 |
| Stage 3 | `independent_reviewer` | 2 | 4 |
| Stage 3 | `methodology_reviewer` | 3 | 4 |
| Stage 3 | `reviewer` | 0 | 1 |
| Stage 4 | `revision_writer` | 6 | 5 |
| Stage 4.5 | `final_paper_evaluator` | 4 | 4 |
