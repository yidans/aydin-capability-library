# Anti-Leakage Protocol (Knowledge Isolation)

**Status**: v3.5
**Used by**: all prose writers (synthesis_writer, longform_expander, editorial_synthesizer, related_work_citation_writer, revision_writer*, abstract_writer) and the formatter/final-evaluator gates.
**Source**: Adapted from PaperOrchestra (Song et al., 2026, Appendix D.4)

---

## BRACKET RULE (the closed-set class rule — HIGHEST PRIORITY)

The ONLY bracketed marker permitted anywhere in the manuscript body is a literature citation
`[refN]` (N a number, e.g. `[ref3]`). EVERY other bracketed token is forbidden — not only the
known internal tags (`[results_csv]`, `[summary_json]`, `[artifact_ref:...]`, `[evidence_ref:...]`,
`[claim_*]`, `[n_metric_*]`, `[MATERIAL GAP]`, `[INSIGHT:...]`) but ALSO any artifact-citation
scheme you might invent (`[A1]`, `[A2]`, `[S1]`, `[data1]`). Never add an "Artifact Citations"
(or similarly named) section. A numeric claim is supported by naming its Table/Figure in prose,
never by a bracketed tag. Enumerating specific forbidden tags is illustrative only; the closed
set — "[refN] allowed, everything else forbidden" — is the rule.

---

## Computational-output leakage (code / JSON in prose) — HIGHEST PRIORITY

The most common machine-generated tell is transcribing the experiment's raw data
structures — JSON keys, boolean literals, arithmetic operators — directly into prose
instead of writing them as academic English. The body must read as if a person wrote it
from the results, never as if a value was pasted from `experiment_results.json`. The
numeric values are exact and MUST be preserved; only their presentation changes from
code to prose.

| Internal pattern (NEVER in prose) | Preferred public-manuscript form |
|------------------------------------|----------------------------------|
| `significant = false` / `significant = true` | `the difference was not statistically significant` / `was statistically significant` |
| `p_value = 0.21`, `p value = 0.21` | `*p* = 0.21` (italic *p*) |
| `effect_size = 0.21`, `cohen_dz = 0.21` | `an effect size (Cohen's *dz*) of 0.21` |
| `n_runs = 40`, `n_levels = 6`, `n_seeds_per_level = 40` | `40 matched pairs`, `six swept levels`, `40 seeds per level` |
| `graph_size_N = 560`, `overall_graphs_total = 520`, ANY `snake_case_key = value` | `560 nodes`, `520 graphs in total`; state the quantity in words, never show the key |
| `40 x 6 x 2 = 520` | `40 seeds across six levels and two conditions (520 graphs in total)`, or × in a factorial label (`a 6 × 2 design`) |
| `[results_csv]`, `[summary_json]`, `[current_report_md]`, `[experiment_script]`, `[reference_index]`, `[study_spec]`, `[design_lock]`, `[review_roadmap]`, `[artifact_ref:...]`, `[evidence_ref:...]`, `[claim_*]`, `[n_metric_*]` | Remove the marker. Use normal table/figure references, section references, and scholarly citations such as `[ref3]` instead. |
| Lowercase sentence/section starts (`the study also fell short`) | Start every sentence with a capital letter |
| Per-seed dumps (`for seed 0 ...; for seed 1 ...`; a table of seeds 0–11 of 40) | Report the aggregate (mean ± SD, n) in one sentence plus an aggregate per-condition/per-level table; cite the full per-run table in the package — never present a truncated sample as the whole |

Write statistics as a person would: `the paired mean difference was 0.0076 (95% CI
[-0.0036, 0.0184]; *p* = 0.21; Cohen's *dz* = 0.21), not statistically significant across
40 matched seed pairs` — never `value = 0.0076, p_value = 0.21, significant = false,
n_runs = 40`.

---

## Purpose

When the user provides comprehensive research materials (RQ Brief, Synthesis Report, Annotated Bibliography, experimental data), the writing agent should construct the paper **primarily from those materials**, not from LLM parametric memory or internal pipeline artifacts. This prevents:

1. **Methodology fabrication (Failure Mode 6)**: The LLM writes a plausible Methods section from training data rather than the user's actual procedure
2. **Implicit knowledge leakage**: The LLM fills gaps with memorized content that may be inaccurate, outdated, or from a different context
3. **Unintentional plagiarism**: The LLM reproduces near-verbatim passages from training data
4. **Process-artifact leakage**: The public manuscript body exposes pipeline state, revision history, package structure, file names, script behavior, or internal reproducibility checks instead of describing the study in normal academic prose. Reproducibility/data/code availability sections may cite released public code or output paths, but must not expose agent internals.

---

## Protocol

### When to activate

Activate when ALL of the following are true:
- The user has provided research materials via the pipeline handoff (RQ Brief + Synthesis Report + Annotated Bibliography)
- The paper is in `full` or `revision` mode (not `plan` or `outline-only`)
- The materials are substantive (not placeholder stubs)

### When NOT to activate

Do NOT activate when:
- The user is in `plan` or `socratic` mode (exploratory — LLM knowledge is expected)
- The materials are minimal (e.g., only a RQ Brief with no bibliography)
- The user explicitly requests the LLM to supplement with its own knowledge

### Prompt insertion

When activated, prepend the following to the draft_writer_agent's working context:

```
## Knowledge Isolation Directive

You are writing this paper based on the research materials provided in this session:
- RQ Brief, Synthesis Report, and Annotated Bibliography (from deep-research)
- Any additional materials the user has provided (experimental logs, datasets, prior drafts)

Priority rules:
1. PREFER session materials over your parametric knowledge for all factual claims
2. Every claim in the paper MUST be traceable to a source in the Annotated Bibliography or user-provided data
3. If the materials do not cover a topic the outline requires, flag it as [MATERIAL GAP] rather than filling from memory
4. Do NOT introduce references not present in the Annotated Bibliography unless explicitly asked by the user
5. The Methods section must describe ONLY what is documented in the user's materials — do not infer or interpolate experimental procedures

This is NOT a prohibition on using language skills or academic writing knowledge.
You may use your knowledge of academic conventions, writing style, logical argumentation,
and discipline norms. The limit applies only to FACTUAL CONTENT — claims, citations,
data, and methodology descriptions must come from session materials.
```

### [MATERIAL GAP] handling

> **Tag vocabulary.** The `[MATERIAL GAP]`, `[WEAK EVIDENCE]`, `[GAP]` tags used throughout this protocol are canonically defined in [`shared/compliance_checkpoint_protocol.md#canonical-gap-tag-vocabulary`](../../shared/compliance_checkpoint_protocol.md#canonical-gap-tag-vocabulary). This section describes how the anti-leakage writing-time flag interacts with that vocabulary during manuscript production.

When a `[MATERIAL GAP]` is flagged:
1. The gap is surfaced at the next checkpoint
2. The user can provide additional materials, or authorize LLM supplementation for that specific gap
3. If supplemented: the gap section is tagged `[LLM-SUPPLEMENTED]` in the draft metadata for integrity review

### Public manuscript hygiene

Knowledge isolation also applies to internal workflow language. The public manuscript body must describe the study, data, methods, and limitations without exposing package mechanics, validator state, or revision state. Reproducibility appendices and data/code availability statements may cite public code/data/output paths when those materials are actually released. Keep agent-internal process evidence, task IDs, checkpoint logs, repair history, and reviewer-response records in process records, audit reports, or reviewer-response documents unless the user explicitly asks for a methods appendix about the AI workflow.

Data and code availability statements must be concrete. When a release package exists, name the released package-relative files or repository/DOI. Do not use empty placeholders (`available at .`, `stored at`) or promotional template language such as `INCLUDED IN FULL` or `no materials are withheld`.

Before final handoff, remove or rewrite these manuscript-level leakage patterns:

| Internal pattern | Why it fails in a submission manuscript | Preferred public-manuscript form |
|------------------|------------------------------------------|----------------------------------|
| `Academic Pipeline Research Package` | Title-page/package label, not a manuscript title | Delete, or replace with author/course/manuscript metadata |
| `specified in the study design` | Sounds copied from a design document | `provide the inputs for the analysis` |
| `analysis run` | Sounds like a code execution log | `when the data were collected` or `during data collection` |
| `revision-stage`, `revised package`, `revised sensitivity program` | Exposes version history and response-to-review state | `sensitivity analyses`, `the analysis`, `the study` |
| `configs/study_spec.json`, `data_notes.md`, `integrity_reports/`, `research/`, `tables/`, `logs/`, `outputs/` in the manuscript body | Exposes package file paths instead of methods, evidence, or reproducibility concepts | `the locked study specification`, `the reproducibility materials`, `the audit record`, `the generated tables` |
| `configs/study\_spec.json`, `data\_notes.md`, `integrity\_reports/`, `research/`, `tables/` in the manuscript body | Same leak after LaTeX escaping | Rewrite as ordinary prose before compiling |
| `implemented package`, `reviewed package`, `realized package`, `package specification`, `package uses`, `package contains` | Makes the manuscript read like an internal package report | `this study`, `this analysis`, `the benchmark`, `the generated data`, `the reproducibility materials` |
| `package does not implement`, `originally envisioned` | Explains an unfinished plan instead of a study limitation | `This study does not evaluate ...` |
| `tract file itself`, `run file`, `retained artifact` | Exposes file mechanics rather than analytic meaning | `the tract-level data`, `the retained data`, `the available outputs` |
| `baseline composite is rebuilt` | Sounds like a code check | `the composite index is recalculated` |
| `implemented measurement bundle`, `implemented thresholds`, `implemented spatial access proxies` | Repeats implementation language where study language is cleaner | `the measures used here`, `the threshold definitions`, `the spatial access measures in this analysis` |

Use `implemented` sparingly and only when implementation is the actual object of study, such as comparing software implementations. In ordinary Methods, Results, and Discussion prose, prefer `used here`, `specified in this analysis`, `estimated`, `measured`, `calculated`, `applied`, or `evaluated`.

---

## Relationship to existing checks

| Check | What it catches | Anti-leakage adds |
|-------|----------------|-------------------|
| Integrity gate (Stage 2.5) | Fabricated citations post-hoc | Prevents fabrication at writing time |
| Failure Mode 6 (Methodology fabrication) | Methods don't match actual procedures | Prevents LLM from inventing procedures |
| Writing Quality Check | AI-typical phrasing patterns | Anti-leakage prevents AI-typical *content* (as opposed to style) |
| Public manuscript hygiene | Pipeline/revision/package wording in the draft | Keeps internal workflow evidence out of submission-facing prose |

---

## References

- Song, Y. et al. (2026). PaperOrchestra. *arXiv:2604.05018*. Appendix D.4 (Anti-Leakage Prompt).
- Lu, C. et al. (2026). Towards end-to-end automation of AI research. *Nature* 651, 914-919. — Mode 6 (Methodology fabrication).

## Governance-vocabulary leakage (added after live review, 2026-07)

The pipeline's own control-plane terms are internal provenance and MUST NOT appear in
manuscript prose or titles: "locked estimand", "estimand lock", "design lock",
"permanent-rewrite criterion", "confirmatory criterion" (as a named object), "reality
gate", "carry-forward", or "evidence packet". Translate them to standard research
English: "the pre-registered primary estimand", "the pre-specified decision rule", or
"the confirmatory hypothesis". Standard scientific terms such as "manipulation check"
and "data provenance" are allowed when they describe the study's actual methods; never
use them as aliases for internal gates, logs, or orchestration state. A reader must not
be able to infer the orchestration system from the paper's vocabulary.
