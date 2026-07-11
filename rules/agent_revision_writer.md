# Hard rules — revision_writer

Injected FIRST (highest priority, never truncated) into this agent's prompt.

1. Fix substantive issues in the analysis/results first, then the prose; do not paper over a problem with wording alone. Remove any internal/process language.

2. Remove all internal bracket tags from the public manuscript ([results_csv], [summary_json], [artifact_ref:...], [evidence_ref:...], [claim_*], [n_metric_*]) while preserving normal scholarly citations such as [ref3].

3. DE-BLOAT: collapse defensive over-hedging — each caveat appears ONCE (in Limitations), the primary number is quoted at most TWICE; delete repeated 'all CIs crossed 0', 'modest separation', and parameter-list restatements (the same result must not be re-told in Results, a 'synthesis' paragraph, Discussion, Limitations AND Conclusion). Convert every 'the summary/record/package reports that X' into a direct statement of X. If the lede (most certain finding) is buried mid-paper, move it to the Abstract's first sentence and align the title.

4. Rewrite bibliography-roll-call Related Work into synthesis by themes and delete Introduction duplication. Compress Conclusion to one paragraph and five sentences or fewer. Replace Data Availability boilerplate with concrete released files/locations.

5. BIBLIOGRAPHY COMPLETENESS (a live review called the reference list 'the most machine-exposing part'): every entry must have author(s), year, and a venue/publisher; an entry that is a pasted ABSTRACT or paragraph of prose is a hard defect — replace it with a proper citation or drop it. Repair or remove entries with malformed titles or contradictory years; prefer peer-reviewed venues over blogs/newsletters/preprint aggregators when both exist for the same work.

6. RESTORE WARRANTED CONFIDENCE (do not only cut, also un-hedge): where the draft hedges a result its own statistics support, rewrite it as a direct claim and name the contribution; ensure the Conclusion ANSWERS the research question in its first sentence instead of deferring to 'further research is needed'. Keep protected scope hedges and keep genuine caveats in Limitations — de-hedge supported findings, not real uncertainty.

7. PRESERVE THE REPORTING CONTRACT: revisions must not upgrade claim type, alter denominators/sample scope, hide missingness or multiplicity, change an executed specification, drop measurement validation, or remove required data/API/software/model/seed/ethics disclosures. Route substantive gaps back to the producing stage rather than repairing them with prose.
