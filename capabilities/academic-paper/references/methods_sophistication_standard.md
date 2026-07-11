# Methods Sophistication Standard (Computational Social Science)

The minimum analysis bar for an empirical paper in computational social
science / science-of-science (calibrated to QSS, JOI, Scientometrics, EPJ Data
Science, ICWSM, and the quantitative sections of Nature Human Behaviour). An
external faculty review of three shipped papers found every one of them below
this bar — "a statistical bakeoff between two periods" — with the same root
pattern: unadjusted group means plus a bootstrap, and survival-shaped questions
collapsed to fixed-window binaries. Raw contrasts are a starting point, never
the finished analysis.

## The core rule: the estimator must match the estimand

1. **Time-to-event outcomes get survival methods.** If the outcome is
   survival, persistence, abandonment, retention, or time-to-repeat, a
   fixed-window binary proportion alone is inadequate. Required alongside it:
   Kaplan-Meier curves per cohort (statsmodels `SurvfuncRight`), and a Cox
   model (statsmodels `PHReg`) or a discrete-time hazard (unit-period logit
   with duration dummies) with the cohort-by-exposure interaction. lifelines
   is NOT installed in the sandbox; statsmodels >= 0.14 is.

2. **Observational contrasts get adjustment.** A two-cohort or two-period
   comparison requires, beyond the raw contrast: one covariate- or
   fixed-effects-adjusted model with pre-registered covariates drawn from
   columns the corpus actually carries (cohort + field + year fixed effects
   count as adjustment; matching or entropy balancing is an accepted
   alternative). The adjusted estimate is registered as a pre-declared family
   member — never a silent replacement of the locked primary.

3. **Every design gets one falsification.** A placebo test appropriate to the
   design: a pre-period cohort contrast that should be null, shuffled
   treatment labels, or a placebo break year (e.g. test 2020/2021 when the
   claimed break is 2023). A "significant" placebo is reported and discussed,
   never hidden.

4. **Inference respects dependence.** Name the cluster unit. When units repeat
   within institutions, fields, or years, use cluster-robust covariance
   (statsmodels `cov_type="cluster"`) or a grouped bootstrap that resamples
   clusters, not rows. A pair bootstrap over 682 pairs drawn from 68
   universities understates uncertainty — a live reviewed defect.

5. **Concentration indices are never compared raw.** Gini/HHI/top-share values
   depend mechanically on sample size and list length. Compare against a
   null-model baseline (e.g. random citing at observed volumes) or use a
   size-corrected index; control for compositional drift (references per
   paper, field volume) before interpreting a trend.

6. **Trends over truncated observation windows are censoring-suspect.** Any
   by-cohort or by-year trend that could be produced mechanically by the
   corpus end date (later cohorts observed for less time) must be checked
   against the censoring bound and either modeled (survival methods, equal
   observation windows) or dropped. A lifespan slope of about -1 year per
   cohort year in a corpus ending at the present is the censoring artifact
   signature.

7. **DiD claims need DiD discipline.** Two units and two periods do not
   support a difference-in-differences label by themselves: show pre-trends
   (with inference, not eyeballing), use an event-study specification with
   leads and lags where the panel allows, and discuss control-group
   contamination (is the "unexposed" field actually unexposed?).

## Reporting duties that come with the methods

- Reconcile your own robustness: a sensitivity row that flips significance
  versus the primary is reported and reconciled in the text, never left
  sitting silently in a table.
- A significant secondary is never buried in an appendix while the main text
  prints dashes for it.
- Methods prose must describe the specification that actually ran — naming
  covariates the executed model does not carry is misreporting.
- Each required analysis lands in its own table under tables/ with estimate,
  SE/CI, and the inference method named.

## Verified infeasibility is honest; silent skipping is not

If the corpus genuinely cannot support a requirement — no covariate columns
beyond the treatment, fewer than 5 clusters, no pre-period events — say so
explicitly in the data provenance and carry it as a named limitation. The
claim of infeasibility must be checkable against the corpus column inventory
and year coverage, not asserted.
