# Universal prose-hygiene rules

Injected into EVERY prose-producing writer (name contains: writer, expander, abstract, manuscript, copyedit, synthes).

1. BRACKET RULE (absolute): the ONLY bracketed marker allowed anywhere in the manuscript is a literature citation [refN] (N a number, e.g. [ref3]). NEVER invent or emit ANY other bracketed marker -- not [A1], [A2], [S1], [data1], [results_csv], [summary_json], [artifact_ref:...], [evidence_ref:...], [claim_*], [n_metric_*], nor any artifact-citation scheme of your own. Support a numeric claim by naming its table or figure in normal prose ('Table 2 reports...'), never with a bracketed tag. NEVER add an 'Artifact Citations' (or similar) section. If you feel the urge to cite an internal artifact, state the fact directly.

2. Write ONLY normal academic prose. NEVER mention or allude to the pipeline, stages, the design lock, the roadmap, the review, 'provided/supplied context', the evidence packet, run files, JSON fields, raw file paths (outputs/..., tables/..., figures/...), an 'executed run/summary/package', a 'local synthetic experiment', a 'research-package archive', 'generated figure metadata', or what information you were or were not given. Internal provenance belongs in data_notes.md and the manifest, NEVER in the manuscript body.

3. STATE FACTS DIRECTLY -- never narrate your own records. Write 'Graphs had 220 nodes', NOT 'the summary reports/records a graph size of 220', 'the result omits/marked', or 'those values are recorded as'. The reader wants the study's facts, not a paraphrase of your log.

4. CONCISION over defensive hedging: state each caveat/limitation FULLY but ONCE (in Limitations); do NOT re-assert it in Methods, Results, Discussion AND Conclusion. Quote the primary numeric result at most TWICE in the whole paper; elsewhere refer to it in words. Over-repeating a caveat reads as self-doubt, not rigor.

5. ONE CANONICAL NAME PER DEFINED QUANTITY: give every window/threshold/cohort definition ONE exact phrase at first use (e.g. 'a two-year window beginning one year after birth') and reuse that phrase VERBATIM everywhere — never alternate namings ('two-year abandonment' in prose vs 'window = 3' in a table) for the same quantity; a live review flagged exactly this as a definitional ambiguity in the core estimand.

6. CALIBRATED CONFIDENCE, not blanket defensiveness: when a result is statistically supported and survives its control, state the finding AND its contribution PLAINLY, in the active voice (e.g. 'larger shared events induced excess clustering not explained by degree'), NOT as 'may possibly suggest a slight tendency'. Be cautious only where caution is genuinely warranted (unsupported, saturated, or underpowered results); reserve hedges for real uncertainty and put scope limits in the Limitations section, once — do NOT self-undermine mid-argument in Results or Discussion. A reliable finding described timidly reads as weaker science, not more rigorous science.

7. NEVER use the pipeline's internal governance vocabulary in the manuscript: 'locked estimand', 'estimand lock', 'design lock', 'permanent-rewrite criterion', 'confirmatory criterion', 'reality gate', 'carry-forward', 'evidence packet', 'manipulation check' (as a phrase), 'data provenance' (as a phrase). Say it in standard research English instead: 'the pre-registered primary estimand', 'the pre-specified decision rule', 'the confirmatory hypothesis', 'cohort separation on the classifying variable'.

8. VARY HEDGING LANGUAGE and cap its repetition: the SAME defensive qualifier (e.g. 'bounded', 'non-confirmatory', 'does not provide confirmatory evidence') may appear at most TWICE in the whole manuscript; each qualifier concept appears at most once per section. Repetition of identical guard phrases reads as machine-generated self-protection, not rigor — state the epistemic status precisely once, then write plainly.

9. PLAIN-ENGLISH TERMINOLOGY, never pipeline labels: condition tokens (C1_*, C2_*), *_proxy suffixes, snake_case column names, and composite artifact nouns (a live paper said 'benchmark-paper unit' throughout because the data manifest used that label — the unit was simply 'a benchmark') must never appear as manuscript terminology. When context.section_contract.terminology provides a glossary, use ITS names for the unit of analysis, the event/observation rows, and the outcome, verbatim and consistently.

10. SOCIAL-SCIENCE NARRATIVE: begin from the phenomenon and analytical tension, not from a generic literature gap. Every Introduction/Related Work/Discussion paragraph must connect one claim to named constructs, population scope, design evidence, and the research question. Avoid 'various factors', 'different aspects', and paper-by-paper bibliography chains.

11. READABILITY RHYTHM: remove mechanical transition chains ('Moreover', 'Furthermore', 'Additionally'), vary sentence openings and lengths, and replace abstract scaffolding with concrete social-science nouns: constructs, cases, datasets, units, mechanisms, outcomes, and estimates.

12. NUMBER FORMATTING: integers of five or more digits take thousands separators in prose, captions, and tables — write 10,498 event rows, not 10498 (a live external review flagged every large count in the paper). NEVER add separators to years (2015-2025), p-values, decimals' fractional parts, identifiers, DOIs, or seeds.
