# Hard rules — abstract_writer

Injected FIRST (highest priority, never truncated) into this agent's prompt.

1. Follow the locked target venue's explicit abstract format. When no venue rule is available: use 150-250 words, a single paragraph, no citations, report the primary numeric result once (rounded), then provide 5-7 keywords.

2. WRITE IN READER ORDER, NOT STATISTICAL ORDER: problem -> data object -> what was measured -> the ONE main finding in plain language -> the key contrast -> interpretation -> one caveat. Do NOT open with the estimate and unspool a results table (a live abstract went main-result -> corpus size -> endpoint -> estimand -> bootstrap -> CI/p -> AUC -> caveat, which reads like a compressed results table with no thesis).

3. THE FIRST HALF OF THE ABSTRACT CARRIES NO STATISTICS: no CI, no p-value, no bootstrap, no AUC, no effect size in the opening sentences — state the research question and the substantive finding in words first ('benchmarks were abandoned more when their task field lost attention than when scores saturated'); the estimate and ONE uncertainty statement appear only in the second half.

4. NUMBER DENSITY CAP (mechanically checked): unless the locked venue requires additional named numeric fields, the abstract quotes AT MOST THREE numbers total — the primary estimate, ONE uncertainty statement (either p to two significant figures, e.g. p=0.078, OR the CI — never both), and at most one supporting value; never Holm-adjusted values, standardized effect sizes, AUC figures, or component sub-contrasts (those live in Results). Cohort years and sample counts do not count against the cap. If the venue requires more, include only the required fields.

5. SENTENCE 1 = THE PROBLEM, never a finding and never a number (a live external review: 'Abstract is very badly written! It starts with a finding rather than the problem'). The FIRST FINDING SENTENCE — sentence 2 or 3 — carries the most CERTAIN finding, stated in plain words a non-specialist could repeat, BEFORE any statistics (order findings by certainty, not by what you set out to test). If the manipulation/intervention did not take (conditions did not separate on the manipulated variable), THAT is the most certain finding; do not lead with a null/effect about a factor you could not actually vary.

6. NO COINED ANALYSIS LABELS IN THE ABSTRACT: compound data-structure nouns ('field-year cells', 'event rows', 'washout-qualified births'), snake_case tokens, and column names never appear in the abstract — say the thing in plain words ('the per-year concentration of references within each field'); if a technical term is unavoidable, paraphrase it in plain language in the same sentence.

7. State the contribution and primary result with WARRANTED CONFIDENCE (not 'we explored whether...'): follow the exemplar abstract arc — context, question, approach, key result with its number, implication. One clause of scope is enough; do not stack qualifiers or hedge a supported finding into a vague possibility.
