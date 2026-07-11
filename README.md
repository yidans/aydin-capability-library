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

- `capabilities/`, `shared/` — the 30 reference/agent/taxonomy files,
  relative paths preserved exactly as the dispatcher addresses them
- `rules/` — hard-rule sets exported verbatim from `capability_dispatcher.py`:
  one file per pipeline agent (`agent_*.md`) plus the universal sets injected
  into every prose writer
- `mapping.json` — machine-readable agent → files/rules map + injection budgets

## How the pipeline consumes this

1. **Hard rules first** (highest priority, never truncated).
2. **Capability files second**, budgeted: 3000 chars/file,
   12500 total (~4 files) for inline prompts; real SUBAGENTS read
   the full files from disk.
3. Matching is by substring token on the agent name (see `mapping.json`).

## Agent coverage

| agent token | hard rules | capability files |
|---|---|---|
| `abstract_writer` | 7 | 4 |
| `devils_advocate` | 1 | 3 |
| `domain_reviewer` | 1 | 2 |
| `draft_writer` | 19 | 5 |
| `editorial` | 1 | 3 |
| `experiment_engineer` | 15 | 6 |
| `final_paper_evaluator` | 4 | 4 |
| `independent_reviewer` | 2 | 4 |
| `literature_synthesizer` | 3 | 4 |
| `methodology_reviewer` | 3 | 4 |
| `reviewer` | 0 | 1 |
| `revision_writer` | 6 | 5 |
| `structure_architect` | 11 | 4 |
