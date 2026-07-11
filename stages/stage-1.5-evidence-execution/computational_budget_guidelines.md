# Computational Budget & Experiment Scale (network / computational social science — GENERAL)

Any experiment you propose (degree-preserving sweeps, SIR/epidemic, opinion dynamics, ABM,
community-detection benchmarks, …) must pass this discipline BEFORE execution. Do NOT apply
fixed magic numbers — reason by AXIS and estimate your own cost first.

**1. Cost model first.** Decompose estimated cost into: per-instance observable complexity ×
number of instances (graphs/realizations) × replication/resampling layers; for dynamics/ABM
add × time-steps × random trajectories per parameter point. State this decomposition and an
order-of-magnitude total in your plan. (This is also how you see the real bottleneck — e.g.
bootstrap on per-seed summaries is cheap; recomputing the observable per resample is not.)

**2. Pre-run feasibility probe (MANDATORY).** Set an explicit wall-clock budget B. Before the
full run, TIME one minimal unit (e.g. 1e4 swaps, or the observable on ONE graph) and
extrapolate to the whole grid. If the estimate > B, SHRINK the scale by the order in §8 FIRST,
then run — never "just run and see if it overruns" (that gets killed mid-way with no results).

**3. Complexity gate.** Determine the observable's big-O. Anything super-linear — betweenness/
closeness & all-pairs/global-efficiency ≈ O(N·m), spectral decomposition O(N³), exact motif
counting — MUST be sampled/approximated (e.g. k-source estimation, k≈200–500) OR run at reduced
N so per-instance cost fits B. Never exactly brute-force a super-linear observable on a large
graph. (Library matters too: pure-Python vs graph-tool differ 10–100×; the probe absorbs this.)

**3b. Manipulation validity — TWO MANDATORY gates (a controlled experiment whose treatment
does not actually differ from its control is worthless before you write a word):**
- **(a) Reachability gate — BEFORE the full run.** Pilot the manipulation on ONE seed to a
  large budget and find the CEILING the manipulated variable can reach (e.g. how high can
  targeted rewiring push transitivity on this sparse degree sequence?). If your target is above
  the ceiling it is UNREACHABLE — lower the target or change the method; do not brute-force.
  This also exposes a strength parameter that is silently not biting (e.g. an MCMC temperature
  whose acceptance ≈ 0.999 against a cost on the scale of one swap's effect → it is just a
  random walk = the control). Set the strength parameter from the COST SCALE of a single step,
  not a habitual constant.
- **(b) Manipulation check — AFTER the run, HARD STOP before any conclusion.** Verify the
  treatment vs control conditions SEPARATE on the MANIPULATED variable (NOT the outcome): the
  realized treatment-minus-control difference must be (i) directionally correct and (ii) larger
  than the within-condition seed-to-seed SD. If they do NOT separate, the experiment manipulated
  NOTHING — a null on the outcome is uninformative and FORBIDDEN to report as a null/effect about
  the manipulated factor. Report "manipulation failed/insufficient" (a methodological finding) or
  increase manipulation strength and re-run. Emit a `manipulation_check` object in
  experiment_results.json: `{manipulated_variable, control_is_randomization,
  per_level:[{level, treatment_value, control_value, within_seed_sd}], separated: bool}`.

**4. Axis classes — manage by class:**
- **Identification & power (SACRED — touch LAST, never cancel):** ≥2 conditions (treatment/
  control); exact matched/paired control where that IS your identification strategy (e.g. an
  exact fixed degree sequence — itself never droppable); enough independent replicates per
  condition for target power (typically ~40; floor from a power analysis, generally not <20–25).
- **Manipulation strength (SACRED — never cut for compute):** the swaps / steps / temperature
  that make the TREATMENT actually DIFFER from control on the manipulated variable. A targeted /
  Metropolis rewire toward a target is NOT a convergence axis — it is the experiment's whole
  point; it must be strong enough that the manipulated variable genuinely separates treatment
  from control (§9b). Set it from whether the manipulation SUCCEEDS, NEVER from how the outcome
  looks ("outcome still interior" is the WRONG reason to stop). Short on compute → cut
  levels/replicates, never starve the manipulation.
- **Convergence — RANDOMIZATION / mixing ONLY — CUTTABLE:** the CONTROL's random-swap mixing /
  MCMC burn-in. Stop by DIAGNOSTIC (autocorrelation / plateau), capped at a small multiple of
  the natural scale (degree-preserving randomization ≈10×m, → 5×m after verifying mixing; NEVER
  100×m). This is the CONTROL knob, NOT the manipulation knob above — do not conflate them.
- **Averaging (null replicates / trajectories per point) — CUTTABLE:** error ∝ 1/√N. ~100 for
  a z-score/point estimate; raise to 200–500 ONLY to resolve an empirical tail p<0.01.
- **Resolution (sweep levels / approximation precision) — CUTTABLE:** default 3–4 levels
  including ONE confirmatory target level; do NOT default to 6+.

**5. Confirmatory vs exploratory.** Full rigour goes to exactly ONE pre-specified confirmatory
test. Exploratory levels/quantities may use cheap generators (soft configuration model /
Chung–Lu, O(m)) and sampled approximations, but their results are DESCRIPTIVE ONLY and must not
support a confirmatory conclusion.

**6. Bootstrap/permutation:** 1000 resamples; negligible on per-seed summary values — do NOT
cut it as a multiplicative layer. If it recomputes the observable per resample, count it as a
multiplier and shrink accordingly.

**7. Checkpoint & graceful degradation.** Write partial results as you go; near B, stop and
honestly report the completed portion.

**8. Over-budget cut order (least loss of rigour first):** ① convergence (10×m→5×m / shorten
steps to just-converged) → ② averaging (replicates/trajectories →~100) → ③ sweep levels →3 →
④ exploratory levels → soft models + sampled observables → ⑤ LAST resort, independent
replicates ~40→~25 (power floor). NEVER drop the ≥2-condition exact matched/paired control.

**9. Honesty & consistency (HARD): the executed design IS the written design.** If you shrank
scale, the manuscript describes the SHRUNK design as "the design" — do NOT report a shortfall
against an unexecuted larger plan, and do NOT show result tables/levels you did not actually
run. If the rigorous version cannot finish within B, either narrow the scientific question's
scope or honestly report "experiment too heavy, did not complete" — NEVER silently weaken the
design and present it as the original plan, and never emit placeholder/garbage results.
