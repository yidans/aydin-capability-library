# Writing Quality Check

## Purpose

A set of writing quality rules extracted from common patterns in AI-generated text. These are **good writing rules** that apply regardless of whether the text was AI-generated or human-written. The goal is better prose, not detection evasion.

> **Design boundary**: This checklist improves writing quality. It is NOT a humanizer. We do not aim to fool AI detectors. We aim to produce clear, precise, varied academic prose.

Reference this checklist during the self-review step of drafting (draft_writer_agent Step 2.7, report_compiler_agent final check), the Stage 4 public-manuscript cleanup pass, and the Stage 5 academic manuscript read-through gate.

---

## Core operational rules (highest-leverage — apply to every draft and revision)

These target the two failure modes seen most: a paper 3× longer than its information content,
and a paper that paraphrases its own logs instead of stating findings.

1. **Concision over defensive hedging.** State each caveat/limitation FULLY but ONCE — it lives
   in Limitations, not repeated across Methods, Results, Discussion AND Conclusion. Quote the
   primary numeric result at most TWICE in the whole paper (in the section that owns it);
   elsewhere refer to it in words ("the difference was small"), never re-print the same
   value/CI/p four or five times. Over-repeating a caveat reads as self-doubt, not rigor. A
   well-hedged paper says each limitation once and trusts the reader to remember it.

2. **State facts directly — no meta-narration of your records.** NEVER write "the summary
   reports/states/notes…", "the package record…", "according to the executed summary/run…",
   "those values are recorded as…". Write the fact: "Graphs had 220 nodes; the attack removed
   the top 8% of nodes by degree", NOT "the summary reports a graph size of 220 and an attack
   fraction of 0.08". The reader wants the study, not a paraphrase of an internal log file.

3. **Order by certainty, not by design intent.** The Abstract's first FINDING sentence carries
   the most CERTAIN finding (the abstract still OPENS with the problem, and the title still
   names the question/phenomenon — see the Abstract Writing Guide). If the intended
   manipulation failed or the effect could not be measured, the headline is THAT (a
   method/observation result), not a null about the intended factor. Do not bury the real
   lede in mid-Results behind soft words like "modest separation". What you set out to test
   ≠ what you most firmly found.

3a. **Distinguish evidence of absence from absence of evidence.** Interpret the estimate and
   confidence interval against a pre-specified smallest substantively meaningful effect when one
   exists. A sufficiently precise interval inside that equivalence region supports no meaningful
   difference; an interval that still includes meaningful effects is inconclusive; an interval
   excluding the null supports a directional difference. Never treat *p* > .05 alone as evidence
   that the groups are equivalent. Report the estimate and interval in every case.

3b. **Reconcile your own robustness; never bury a significant secondary.** If any
   pre-registered sensitivity/window row flips significance versus the primary, the text
   must report and reconcile it. A significant secondary never sits in an appendix while
   the main text prints dashes for it.

4. **Proofread before submission (mechanical).** No empty filled fields ("recorded at ."); no
   literal markdown marks left in the rendered body (`*p*`, `*dz*`, `**bold**` must render as
   italics/bold, not print as asterisks); no truncated labels/captions ending mid-word; no two
   titles concatenated without a space; no "the summary/record reports" meta-narration surviving.

5. **No internal evidence tags in public prose.** Do not print bracketed provenance markers such
   as `[results_csv]`, `[summary_json]`, `[current_report_md]`, `[experiment_script]`,
   `[reference_index]`, `[study_spec]`, `[design_lock]`, `[review_roadmap]`,
   `[artifact_ref:...]`, `[evidence_ref:...]`, `[claim_*]`, or `[n_metric_*]`. They are internal
   trace handles, not citations. Numeric findings should be supported through normal table,
   figure, and scholarly citation references.

6. **Respect section jobs.** Related Work synthesizes prior work by problem axis rather than
   listing papers one by one. Discussion interprets the results. Limitations state caveats once.
   Conclusion closes the paper in one paragraph and no more than five sentences; it does not
   replay the Introduction, Related Work, Discussion, or Limitations.

## Stage 5 Academic Manuscript Read-Through Gate

Before final export, run a whole-manuscript academic read-through. This pass should be performed as a content-preserving editor/reviewer judgment, not as a hard keyword validator. The question is whether the public text reads like a scholarly manuscript that could be submitted, not whether it merely avoids a fixed list of forbidden strings.

### Required Review Questions

1. Does the manuscript stand alone as a research paper, or does it still sound like an internal package report, process record, validation memo, or revision log?
2. Do the Methods explain the study design, data, measures, assumptions, and analysis choices as research methods rather than as file paths, scripts, package mechanics, or implementation bookkeeping?
3. Do the Methods cite external data sources, data documentation, public APIs, measurement instruments, statistical tests, estimators, diagnostics, algorithms, benchmark definitions, and methodological software/tool documentation when used?
4. Do Results, tables, and figures appear in the body with scholarly interpretation rather than references to generated artifacts?
5. Does every figure/table caption describe the actual visible variables, axes, rows, columns, groups, statistics, and comparisons?
6. Does the surrounding prose make only the claim that the figure/table actually supports?
7. Are limitations framed as inferential, measurement, sampling, construct-validity, or external-validity limitations rather than as notes about package features not implemented?
8. Are provenance, hashes, validators, checkpoints, subagents, logs, and repair history kept out of the public manuscript body, with code/data/output paths limited to genuine reproducibility, data availability, or code availability sections?
9. Are claims proportionate to the evidence, without treating audit or validator status as a substantive finding?
10. Are statistical results written as prose rather than transcribed code/JSON? No raw key=value or boolean fields (`significant = false`, `p_value = 0.21`, `n_runs = 40`, `graph_size_N = 560`), no programmatic operators (`40 x 6 x 2 = 520`); *p*-values and effect sizes italicized; every sentence starts with a capital. (See anti_leakage_protocol.md → "Computational-output leakage".)
11. Are tables aggregate (per condition/level) with complete rows, rather than truncated per-seed dumps (e.g. seeds 0–11 of 40) presented as the full table? Are repeated headline statistics stated in full once (Results) and referred to in words elsewhere, not re-quoted in every section?
12. Does the manuscript contain zero internal bracket tags such as `[results_csv]`, `[summary_json]`, `[artifact_ref:...]`, `[evidence_ref:...]`, `[claim_*]`, or `[n_metric_*]` while preserving legitimate scholarly citations?
13. Is the Related Work synthetic rather than a bibliography roll call, and does it avoid duplicating the Introduction's paper-by-paper framing?
14. Is the Conclusion one paragraph and no more than five sentences, without re-running Discussion, Limitations, or future-work lists?
15. Does the Data/Code Availability statement name concrete released locations or package-relative files and avoid empty placeholders or slogans such as "no materials are withheld"?
16. Are bibliography entries deduplicated by DOI or normalized URL?
17. Do figure/table titles and captions describe the actual variables and design? In particular, a swept treatment-level or effect-by-level figure must not be called a "sensitivity analysis" unless the plot varies an analysis assumption or robustness setting.

### Required Output

Write the internal review result to:

- `integrity_reports/public_manuscript_academic_prose_review.md`
- `logs/stage_5_finalization_report.json` `checks.academic_prose_read_through`

The Stage 5 finalization report must use `"verdict": "PASS"` for `checks.academic_prose_read_through` only when all checks below pass and `blocking_issues` is empty:

- `standalone_scholarly_register`
- `internal_report_language_absent`
- `methods_are_study_facing`
- `methods_citation_sufficiency`
- `data_source_citations_present`
- `statistical_method_citations_present`
- `limitations_are_study_facing`
- `figures_integrated_scholarly`
- `figure_caption_alignment`
- `figure_caption_no_unshown_variables`
- `statistics_in_prose_not_code`
- `tables_aggregate_not_truncated_dumps`
- `headline_stats_stated_once`
- `internal_provenance_tags_absent`
- `related_work_synthesis_not_roll_call`
- `conclusion_short_closing`
- `data_availability_concrete`
- `references_deduplicated`
- `figure_titles_match_plotted_design`
- `figure_prose_claim_alignment`
- `tables_caption_alignment`
- `rendered_pdf_tables_not_clipped`
- `claims_proportionate`

`figures_integrated_scholarly` is not satisfied by presence alone. It requires the figure image, caption, and surrounding prose to match the same evidence.

If the first read-through fails, revise the manuscript wording/register and repeat the read-through. Do not silently change verified scientific content. Do not delegate this decision to the final code validator; the validator may confirm that the gate ran, but the academic-register judgment belongs to this prose review.

---

## Stage 5 Repair-Before-Fail Addendum

Stage 5 must repair before it fails. If the manuscript is below the locked word-count range, has zero or sparse in-text citations, lacks data-source or statistical-method citations, leaves generated figures/tables outside the public manuscript, fails `scripts/verify_figure_table_embedding.py`, has caption/prose/render mismatches, or fails final-format parity, revise the canonical manuscript source and regenerate outputs before writing `FAILED_STATUS.md`.

Required repair evidence:
- `logs/stage_5_repair_attempts.jsonl`
- `logs/stage_5_finalization_report.json`
- `integrity_reports/stage_5_summary.md`

Citation repair must inventory Methods/source items, record the locked package minimum reference count from the design-lock/intake config, and add nearby in-text citations plus References entries for external datasets, APIs, data documentation, measurement definitions, statistical methods, diagnostics, software, and literature claims. The final References section must meet that locked reference floor. Figure/table repair must enumerate every generated figure/table and either integrate it into Markdown/LaTeX/PDF or record why it supports no public manuscript claim. For included public evidence, every figure under `figures/` and analytical CSV under `tables/` must be embedded in `report/report.md` and `report/report.tex` with caption plus in-text reference; a loose artifact that does not appear in the LaTeX manuscript source is Stage 5 FAIL.

When PDF is requested, Stage 5 must also record `checks.pdf_render_verification` in `logs/stage_5_finalization_report.json`. PASS requires zero render warnings: no clipped or overflowed tables, no unreadable table columns, no blank pages, no unresolved references/citations, and no Overfull/Underfull box or LaTeX warning in the compile log.

---

## Public Manuscript Provenance Check

Fail the writing quality sweep if the public manuscript contains internal workflow language unless the paper is explicitly about the software system itself.

Flag and rewrite:
- package
- artifact
- exported
- repaired
- Stage
- pipeline
- workflow
- checkpoint
- manifest
- validation run
- operator instruction
- internal draft
- provenance

Standard scientific uses such as `data provenance` are allowed when they describe
the study's actual data lineage or methods. Flag only workflow provenance that
exposes internal orchestration, validation, or revision state.

Acceptable public alternatives:
- study
- analysis
- benchmark
- results
- diagnostic analysis
- sensitivity analysis
- revised specification, only if discussing methodology history is explicitly necessary

The public paper must not read like an internal report. The manuscript body should describe the scientific method, analysis, and results without path provenance. Reproducibility appendices, data availability statements, and code availability statements may cite public code/data/output paths when those files are released, but they must not expose agent internals such as task IDs, subagents, checkpoints, Stage labels, reviewer-response history, repair-process wording, operator instructions, or validation-gate narration.

---

## A. High-Frequency Term Warnings

The following terms appear disproportionately in AI-generated text. They are not banned — but when you encounter one, ask: **"Is this really the most precise word here, or am I defaulting to it?"**

### Flagged Terms

| Term | Why it's flagged | Better alternatives (context-dependent) |
|------|-----------------|----------------------------------------|
| delve | Overused as "explore" substitute | examine, investigate, analyze, explore |
| tapestry | Cliché metaphor for complexity | network, interplay, system, landscape |
| landscape | Vague when not literal | field, domain, context, state of |
| pivotal | Inflation of importance | important, significant, central, key |
| crucial | Same as above | essential, necessary, critical, vital |
| foster | Vague verb | promote, develop, cultivate, encourage |
| showcase | Non-academic register | demonstrate, illustrate, present, reveal |
| testament | Cliché | evidence, indicator, demonstration |
| navigate | Vague when not literal | manage, address, handle, negotiate |
| leverage | Business jargon | use, employ, utilize, apply |
| realm | Archaic/poetic | domain, field, area, sphere |
| embark | Overwrought for "begin" | begin, initiate, undertake, start |
| underscore | Overused emphasis verb | emphasize, highlight, stress, reinforce |
| multifaceted | Vague complexity claim | complex, varied, diverse, multilayered |
| nuanced | Often vacuous | subtle, detailed, fine-grained, qualified |
| comprehensive | Often unjustified | thorough, extensive, broad, detailed |
| robust | Vague quality claim | reliable, strong, rigorous, resilient |
| intricate | Same problem as multifaceted | complex, detailed, elaborate, involved |
| cornerstone | Cliché metaphor | foundation, basis, core element, pillar |
| paradigm | Overused outside philosophy of science | framework, model, approach (exception: "paradigm shift" in philosophy of science is standard) |
| synergy | Business jargon | interaction, cooperation, combined effect |
| holistic | Vague without definition | comprehensive, integrated, whole-system |
| streamline | Non-academic | simplify, optimize, improve efficiency |
| cutting-edge | Cliché | recent, advanced, state-of-the-art, novel |
| groundbreaking | Inflation | novel, innovative, pioneering, original |

### Exception Rule

If a flagged term is **standard terminology in the target discipline**, it is exempt:
- "paradigm shift" in philosophy of science → OK
- "landscape" in ecology/geography (literal) → OK
- "robust" in statistics ("robust estimator") → OK
- "navigate" in wayfinding research (literal) → OK

---

## A2. Internal Workflow Language Warnings

The final manuscript must not read like a generated package, execution log, or response-to-review memo. The following phrases are not style preferences; they are public-output hygiene flags. Rewrite them unless the target document is explicitly a process record, audit report, README, or reviewer response.

| Flagged wording | Preferred revision pattern |
|-----------------|----------------------------|
| `Academic Pipeline Research Package` | Remove from title page, or replace with author/course/manuscript metadata |
| `specified in the study design` | `provide the inputs for the analysis`; `were used in the analysis` |
| `analysis run` | `when the data were collected`; `during data collection`; `in this analysis` |
| `Revision-stage sensitivity analyses` | `The sensitivity analyses` |
| `baseline composite is rebuilt` | `the composite index is recalculated` |
| `tract file itself` | `the tract-level data`; `the available tract records` |
| `package does not implement` | `This study does not evaluate`; `This analysis does not include` |
| `originally envisioned` | Remove; state the actual limitation without version history |
| `In the revised package` | Remove; start with the substantive claim |
| `The revised sensitivity program` | `The sensitivity analyses` |
| `original predictor-specification checks` | `Additional predictor-specification checks` |
| `implemented measurement bundle` | `the measures used here`; `the measurement set used in this analysis` |
| `implemented thresholds` | `the threshold definitions`; `the thresholds used here` |
| `implemented spatial access proxies` | `the spatial access measures`; `the access proxies used in this analysis` |
| `implemented index` | `the index used here`; `the calculated index` |

Acceptable uses of `implementation` language are narrow: software-methods papers, reproducibility appendices, or technical sections where code implementation is the object under evaluation. Even there, avoid repeating `implemented` as a generic adjective.

---

## B. Punctuation Pattern Control

### Em Dash (—)
- **Review trigger**: repeated em dashes used as a default parenthetical device
- **Why**: Frequent asides can obscure the main clause; there is no universal numeric cap, and venue style takes priority
- **Fix**: Keep purposeful uses; otherwise use commas, parentheses, or separate sentences
- **Exception**: Direct quotes from sources retain their original punctuation

### Semicolons
- **Review trigger**: repeated semicolons chaining sentences that would read more clearly apart
- **Why**: Semicolons are legitimate academic punctuation, but repeated clause chaining can become dense
- **Fix**: Keep semicolons that express a close relationship or separate complex list items; split only when clarity improves

### Colon-List Sequences
- **Rule**: Avoid 2+ consecutive paragraphs that each open with a colon followed by a list
- **Why**: Creates a monotonous enumerate-everything pattern
- **Fix**: Integrate list items into prose, or use a single consolidated list

---

## C. Throat-Clearing Openers

Delete the following sentence starters. Cut to the point.

| Throat-clearing phrase | What to do |
|-----------------------|-----------|
| "In the realm of..." | Delete. Start with the actual subject |
| "It's important to note that..." | Delete. If it's important, the content speaks for itself |
| "It is worth mentioning that..." | Same as above |
| "In today's rapidly evolving..." | Delete. Timestamped clichés add no information |
| "This serves as a testament to..." | Replace with direct claim: "This demonstrates..." or just state the evidence |
| "It goes without saying that..." | If it goes without saying, don't say it |
| "In order to..." | Replace with "To..." |
| "It should be noted that..." | Delete. Just note it |
| "As a matter of fact..." | Delete. State the fact |
| "When it comes to..." | Replace with the subject directly: "X shows..." |
| "At the end of the day..." | Delete. Colloquial and vague |
| "With that being said..." | Delete or use "However" if a contrast is intended |

### Meta-Commentary to Avoid

Also watch for sentences that describe what the paper is doing instead of doing it:
- "This section will discuss..." → Just discuss it
- "The following paragraph examines..." → Just examine it
- "We now turn our attention to..." → Just turn to it

Exception: Roadmap sentences in the Introduction ("Section 2 reviews the literature; Section 3 describes the methodology") are standard academic practice and should be kept.

---

## D. Structure Pattern Warnings

### Rule of Three Compulsion
- **Pattern**: Every argument has exactly 3 sub-points, every list has exactly 3 items
- **Why**: Real analysis doesn't always decompose into trios. Two strong points beat three padded ones
- **Fix**: Use as many points as the evidence warrants. 2 is fine. 5 is fine. Don't pad to 3

### Uniform Paragraph Length
- **Pattern**: All paragraphs are approximately the same length (150-200 words each)
- **Why**: Natural writing has paragraph length variation. Short paragraphs for emphasis, longer ones for complex arguments
- **Fix**: Vary paragraph length. A 2-sentence paragraph after a 10-sentence paragraph creates rhythm

### Synonym Cycling
- **Pattern**: Using 3+ different synonyms for the same concept within one paragraph to avoid repetition
- **Why**: In academic writing, consistent terminology is a virtue. Swapping "students" → "learners" → "participants" → "subjects" within one paragraph confuses rather than impresses
- **Fix**: Pick one term per concept per section. Repeat it. Technical repetition is clarity, not weakness

### Binary Contrast Overuse
- **Pattern**: "Not X. Y." or "It's not about X — it's about Y." used more than twice per paper
- **Why**: This rhetorical device is effective once. Repeated, it becomes a tic
- **Limit**: ≤ 2 per paper

### Mirror Structure
- **Pattern**: Every section has the same internal structure (topic sentence → 3 evidence points → synthesis sentence)
- **Why**: Creates a template-stamped feel. Different sections serve different purposes and should have different internal rhythms
- **Fix**: Let section structure follow content needs. Methods can be procedural. Discussion can be exploratory

---

## E. Burstiness (Sentence Length Variation)

### What to Check
Good writing has **natural variation in sentence length**. Short sentences create impact. Longer sentences develop complex ideas. The alternation creates rhythm.

### Detection Rule
If 5+ consecutive sentences all fall within a narrow word-count range (e.g., all between 20-25 words): **flag for review**.

### How to Fix
- Insert a short sentence (≤ 10 words) to break the pattern
- Combine two short sentences into one complex one if the pattern is monotonously short
- Read the paragraph aloud — if it feels metronomic, vary it

### Burstiness Targets (by section)
- **Abstract**: Moderate variation (factual, steady pace)
- **Introduction**: High variation (hook with short sentences, build with long ones)
- **Literature Review**: Moderate (steady analytical pace, occasional short synthesis)
- **Methods**: Low variation acceptable (procedural sections naturally have uniform length)
- **Results**: Moderate (short for key findings, longer for detailed descriptions)
- **Discussion**: Highest variation (short for emphasis, long for interpretation, very short for conclusions)

---

## How to Use This Checklist

### During Drafting (Preferred)
Apply rules **while writing each section** in the self-review sub-step (Step 2.7 in draft_writer_agent). This catches issues before they propagate.

### During Final Review (Fallback)
If not applied during drafting, run a full-paper sweep before handoff to citation_compliance_agent.

### Scoring (Internal, Not Reported to User)
For each rule category, track violations:
- 0 violations: Clean
- 1-3 violations: Minor — fix in self-review
- 4+ violations: Pattern issue — review the section's writing approach

Do NOT report scores to the user. Just fix the issues silently during drafting.

## Readability contract (added 2026-07 after external readability reviews)

1. **Methods definition block**: the first Methods paragraph defines, in plain
   scientific English, the unit of analysis, what one event/observation row
   records, the outcome measure per unit-period (including how missing periods
   are treated), and the observation window — before any procedure detail.
2. **Results raw evidence first**: the first Results paragraph gives raw
   quantities (per-condition unit counts, raw group means/risks) before any
   derived statistic; then the contrast with CI and p.
3. **Abstract number density**: unless the locked venue requires additional
   named numeric fields, use at most three numbers — the group values (or the
   primary estimate) plus ONE uncertainty statement (p to two significant figures
   OR the CI, never both, never adjusted-p/effect-size stacks). If the venue
   requires more, include only the required fields.
4. **Conclusion**: one short paragraph — its first sentence answers the research
   question, and a later sentence may say why the measurement design matters. No
   sentence over ~35 words.
5. **Terminology**: use the section contract's glossary names for unit/event/
   outcome verbatim; never pipeline/data labels (C1_*, *_proxy, snake_case
   columns, composite nouns like "benchmark-paper unit").
6. **Tables**: never hand-number captions ("Table 1. ..."); never reference or
   paste row dumps ("showing the first N of M rows") — full data tables render
   in the appendix, the main text gets ONE clean analytic table at most beyond
   the primary summary.
7. **Related Work**: three argument axes at most, each paragraph ends with the
   author's own evaluative judgment; distant analogies (e.g. hardware lifespans
   for a benchmark-lifecycle paper) go to a footnote or get cut.
