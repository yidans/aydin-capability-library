# Computational Social Science Reporting Card

For empirical computational social science. Report only executed evidence.

## Core contract

1. **Claim-design fit.** Label claims descriptive, associational, causal,
   predictive, or simulated. Causal verbs require identification; prediction and
   simulation do not explain real behavior by themselves.
2. **Sample flow.** Report population, snapshot date, starting records,
   exclusions/deduplication, missingness, final sample, denominators, and
   independent units/clusters.
3. **Measurement.** Define constructs/proxies. For annotation, classifiers,
   extraction, or LLM coding, report validation sample, reference standard,
   agreement/performance, and likely error.
4. **Specification.** Report outcome/estimand, estimator/link, covariates/fixed
   effects, weights/transformations, dependence correction, and missing-data
   method. Match the executed model.
5. **Evidence.** Report estimate/effect, uncertainty or posterior summary, sample
   size, and substantive scale; *p* only when relevant. Label primary, secondary,
   and exploratory analyses and name multiplicity control.
6. **Robustness.** Report pre-specified sensitivities, missing-data checks,
   falsification/placebos, and changes in sign, magnitude, or uncertainty. Never
   select only favorable specifications.
7. **Reproducibility and ethics.** Report data/API snapshot, software/model/prompt
   versions, seeds/runs, and public code/data. State privacy, consent, platform,
   or harm constraints that bound the study.

## Conditional details

- **Networks:** boundary, node/edge construction, direction/weight, projection,
  temporal aggregation, dependence, null/permutation model.
- **Text/ML/LLM:** labels, split/leakage control, class balance, calibration,
  domain shift, model/prompt version.
- **Simulation/ABM:** parameter source, calibration, seeds/runs, stochastic
  uncertainty, sensitivity, external validation.
- **Longitudinal/events:** time origin/risk, censoring, equal windows, temporal
  order, repeated-unit clustering.
