# Exemplar Writing Standard — learn the FORM of strong empirical papers

Used by `structure_architect_agent` and `draft_writer_agent`. This is not vague
"write like a paper" advice: it is the concrete structure, argument rhythm, and
figure/table presentation that strong empirical papers in network science /
computational social science share. **Emulate this FORM and standard — the section
organization, pacing, and presentation — never any exemplar's content or topic.**

## Section organization (what each section must do)

- **Abstract** — one paragraph: context -> question -> approach -> the key result WITH its number -> implication. After the problem and question are clear, lead the findings portion with the key finding rather than restating the aim.
- **Introduction** — a funnel: (1) the problem and why it matters, (2) what prior work establishes and the SPECIFIC gap, (3) this study's question and its stated contribution. End the Introduction knowing exactly what the paper adds.
- **Related Work** — synthesis by argument axis (mechanism, confound/control, method precedent, gap), not a one-paper-per-sentence roll call. Say what each cited work SPECIFICALLY did/found and how it relates to the present design.
- **Methods** — reproducible: design -> data/generative procedure -> measures/estimand -> analysis and tests. A reader could re-run it. Report the control/null explicitly.
- **Results** — lead with the PRIMARY estimand, effect, and appropriate uncertainty (*p* when relevant), then supporting patterns and robustness across variants. Every number ties to a table/figure. Report honestly, including non-monotonic or partial patterns.
- **Discussion** — interpret the finding: what it means, a plausible mechanism only when supported by the design or prior evidence, agreement/tension with prior work, competing explanations, and the bounded scope. Do NOT restate Results number-by-number or convert association into causation.
- **Limitations** — the single home for caveats. State each once, concretely.
- **Conclusion** — ONE paragraph, <=5 sentences, whose FIRST sentence ANSWERS the research question with warranted confidence. At most one future-work sentence.

## Argument rhythm (per section, per paragraph)

- Each section opens with a topic sentence stating its claim; paragraphs then run claim -> evidence (numbers/citations) -> interpretation -> bounded scope.
- One idea per paragraph; paragraphs of 3-6 sentences. Let the number carry the point ("the contrast was 0.072, 95% CI [0.071, 0.073]"), not adjectives.
- Do not re-tell the same result in Results, a "synthesis" paragraph, Discussion, Limitations AND Conclusion. Quote the primary number at most twice.

## Figure / table presentation

- Every figure/table is referenced in the text exactly where its evidence is used, and its point is stated in prose ("Table 1 shows excess clustering positive at every level").
- Show distributions/spread with CIs (box/violin/per-unit scatter), not one bar per condition. Captions are self-contained and name the plotted variables/axes/groups.
- Table columns report the estimand, its uncertainty (CI/SE), and n; no internal/debug columns.

## Confidence calibration (the writing standard, not just hygiene)

- Be cautious where caution is genuinely warranted (unsupported, saturated, or underpowered results). But when a result is statistically supported and survives its control, STATE IT AND ITS CONTRIBUTION PLAINLY, in the active voice.
- Do not down-tone a supported finding to "may possibly suggest a slight tendency", and do not self-undermine mid-argument. Reserve hedges for real uncertainty; put scope in Limitations.
- A reliable finding described timidly reads as weaker science, not more rigorous science.

## Model excerpts (form only — do not copy content)

**Introduction close (gap -> contribution):** "Prior work shows projection is not a neutral step, but it has not isolated how much closure survives a degree-preserving control as event size grows. This study measures that excess directly in a controlled design, contributing a paired, degree-controlled estimate of projection-induced clustering."

**Results lead (claim -> number -> support):** "Excess clustering was positive at every event size. The high-minus-low contrast was 0.072 (95% CI [0.071, 0.073]; one-sided permutation p < 1e-4), and its direction held in 4/4 scenario variants."

**Conclusion first sentence (answers the question):** "In these networks, larger shared events induced one-mode clustering beyond what the projected degree sequence explains — a representation effect analysts should not read as direct triadic closure."

## Titles and hedging economy (added after live review, 2026-07)

- Titles name the QUESTION or PHENOMENON, never the verdict about your own analysis.
  Draft three candidates in different patterns and pick the one a human scientist would
  choose: question-form ("Did Pandemic-Era Remote Work Create Durable Long-Distance
  Collaborations? Evidence from OpenAlex"); concept-form, leading with the Discussion's
  own conceptual one-liner ("Formation Is Not Persistence: Tie Survival After the
  Forced-Remote Period"); or phenomenon+design ("Remote-Born Scientific Ties After the
  Pandemic: A Cohort Survival Analysis"). Real failures, do not imitate: a stacked
  negative verdict ("No confirmatory evidence that...") and reviewer jargon as a
  storefront ("A Floor-Saturated Test of..."). Analysis meta-language ("test of",
  "no evidence that", "bounded", "confirmatory") never appears in a title; at most one
  qualifier; the verdict belongs in the abstract.
- Hedging economy: state the epistemic status of the finding precisely ONCE, then write
  plainly. The same defensive qualifier ("bounded", "non-confirmatory") appears at most
  twice in the whole paper; identical guard phrases repeated across sections read as
  machine self-protection, not rigor.
- Related Work paragraphs end with the author's own evaluative judgment (what the thread
  establishes, what it leaves unsettled for THIS study) — never with the last cited
  paper's finding.
