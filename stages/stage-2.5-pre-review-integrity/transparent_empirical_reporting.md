# Transparent Empirical Reporting

## Purpose

Keep empirical manuscripts auditable from the paper alone: can a reader tell
what was sampled, measured, compared, excluded, and inferred?

## Required Reporting Moves

1. **Source, population, and sampling frame.** Say WHAT each data source is in
   one plain sentence ("OpenAlex, an open bibliographic database indexing
   scholarly works and institutional affiliations"), WHY it fits this
   question, the unit of analysis, HOW records were selected/queried, and why
   this corpus size. Name the external-validity limit when the reachable
   frame differs from the theoretical population.

2. **Observation window.** Exact dates or fixed follow-up windows. Cohort
   comparisons need equal time-at-risk unless a survival/censoring model
   handles unequal exposure.

3. **Measurement and coding.** Define the outcome, predictors, covariates,
   exclusions, and missing-data handling in prose, with public-facing names
   (no snake_case field names).

4. **Primary, secondary, exploratory.** Label confirmatory claims separately;
   never turn an exploratory pattern into a confirmatory headline.

5. **Raw evidence before adjusted evidence.** Group counts and raw contrasts
   before coefficients — the reader sees the two quantities being compared
   before any model output.

6. **Uncertainty travels with numbers.** Every headline estimate carries its
   CI, SE, bootstrap interval, N, or stated precision limit.

7. **Specification transparency.** Name the estimator, clustering, weights,
   fixed effects, and transformations — once, clearly. Methods prose must
   describe the specification that actually ran: naming covariates the
   executed model does not carry is misreporting.

8. **Absence versus uncertainty.** Interpret the estimate and interval against a
   pre-specified smallest substantively meaningful effect when one exists. A
   sufficiently precise interval inside that region supports no meaningful
   difference; an interval that includes meaningful effects is inconclusive; an
   interval excluding the null supports a directional difference. Never treat
   *p* > .05 alone as equivalence.

9. **Robustness reconciled.** A sensitivity row that flips significance
   versus the primary is reported and reconciled in the text; a significant
   secondary is never buried in an appendix behind dashes.

10. **Deviation discipline.** If the executed analysis differs from the plan,
    give the public methodological reason and its inferential effect — never
    pipeline language.

11. **Data and code availability.** Name concrete released files AND the
    public link/DOI of the source corpus; no empty slogans.

## Reviewer Questions

- Can the Methods answer "why this N, sampled how, over what window?"
- Can a reader reconstruct the denominator for every major percentage?
- Does each causal or comparative verb match the design actually executed?
- Does each null or uncertain result reflect interval precision and the
  substantively meaningful-effect threshold rather than *p* > .05 alone?
- Are limitations framed as sampling, measurement, construct, model, or
  external-validity limits rather than as missing implementation features?
