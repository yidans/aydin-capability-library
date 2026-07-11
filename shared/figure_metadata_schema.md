# Figure Metadata Package Schema

Authoritative schema for `figures/<figure_id>.meta.json` files produced by
`evidence_execution_agent` and reviewed by `visualization_agent` (Mode 2).

This schema is the single source of truth for figure metadata. Both producers
and consumers of these files MUST conform.

## Required files per planned figure

For each `figure_id` in `configs/study_spec.json.figures_planned[]`,
`figures/` must contain three artifacts:

| File | Format | Purpose |
|---|---|---|
| `figures/<figure_id>.png` (or `.pdf`) | Rendered image | The figure itself |
| `figures/<figure_id>.meta.json` | JSON object | Machine-readable metadata, schema below |
| `figures/<figure_id>.caption.md` | Markdown | APA-7 caption text |

## meta.json schema

Required fields:

| Field | Type | Description |
|---|---|---|
| `figure_id` | string | Matches the planned `figure_id` exactly |
| `file` | string | Relative path to the rendered figure (e.g. `figures/fig_01.png`) |
| `source_table` | string | Relative path to the analysis table this figure plots (e.g. `tables/main_results.csv`). If the figure plots data not in a table, use `"inline"` and add a `source_inline_description` field. |
| `chart_type_actual` | string | Chart type as actually rendered. Must be a value from `shared/taxonomies/chart_types.py` (TRIVIAL or NON_TRIVIAL). |
| `chart_type_proposed` | string | The `chart_type_proposed` from the matching `figures_planned` entry. Verbatim copy. |
| `chart_type_deviation` | string | `"none"` if `chart_type_actual == chart_type_proposed` (after normalization). Otherwise a non-empty rationale explaining why the chart type was changed. |
| `addresses_visual_question` | string | The `visual_question` from the matching `figures_planned` entry. **Verbatim copy** - validator does exact string match. |
| `x_axis` | string | x-axis variable name and units, e.g. `"diffusion regime (categorical)"` |
| `y_axis` | string | y-axis variable name and units. For multi-panel layouts where panels share axes, describe the shared axes here. |
| `groups_or_legend` | array of strings | Series/groups encoded by color, shape, or panel. Empty array `[]` if the figure has only one series. |
| `statistics_shown` | array of strings | Statistical elements actually rendered, e.g. `["mean", "95% CI", "regression line"]` |
| `not_shown` | array of strings | Variables or comparisons the manuscript might reasonably want from this figure but which it does NOT show. Used by integrity audit to prevent over-claiming. |
| `safe_caption_claim` | string | The strongest claim the figure actually supports, in one sentence. Captions and Results prose must stay within this claim. |
| `claims_not_supported` | array of strings | Explicit "this figure does not show X" statements, parallel to `not_shown` but written as user-facing sentences. Used by Stage 5 alignment check to detect over-claims in caption or prose. |
| `panel_row_labels_rendered` | boolean | True if `panel_layout.rows[*].row_label` from the design spec was visually drawn on the figure. False is allowed only when `panel_layout.rows` was not specified in the spec. |
| `color_scale_saturation_check` | object | Required when `panel_layout.color_scale_handling == "linear"` and multi-panel. Fields: `{min_panel_range: float, max_panel_range: float, ratio: float, mitigated_by: "none" \| "log" \| "quantile_clip_5_95" \| "per_panel_scale" \| "..."}`. |
| `panel_layout_actual` | object | Mirrors `panel_layout` from the spec with any deviations recorded. Empty/missing row_label here when spec has it = render-vs-spec mismatch = FAIL. |

Optional fields:

| Field | Type | Description |
|---|---|---|
| `source_inline_description` | string | Required if `source_table == "inline"`. Describes where the plotted values came from (e.g. "computed in run_analysis.py:152 from outputs/raw_runs.csv aggregates"). |
| `deviation_from_spec` | string | Required when implementation deviates from `notes/figure_design_specs.md` for this figure on any axis other than chart type (which is covered by `chart_type_deviation`). |
| `vlm_verification` | object | Optional VLM verification record (see `capabilities/academic-paper/references/vlm_figure_verification.md`). |

## Example

```json
{
  "figure_id": "fig_01_partition_overlay",
  "file": "figures/fig_01_partition_overlay.png",
  "source_table": "tables/community_assignments.csv",
  "chart_type_actual": "graph with partition overlay",
  "chart_type_proposed": "two-panel force-directed layout",
  "chart_type_deviation": "Implemented as a single force-directed layout with side-by-side panels using identical node positions, per notes/figure_design_specs.md Mode 1 design.",
  "addresses_visual_question": "How do Louvain and Leiden assignments diverge for the same graph?",
  "x_axis": "spatial layout (force-directed, fixed seed)",
  "y_axis": "spatial layout (force-directed, fixed seed)",
  "groups_or_legend": ["Louvain communities", "Leiden communities"],
  "statistics_shown": ["community membership", "node degree (encoded as size)"],
  "not_shown": ["modularity scores", "convergence iterations", "edge weights"],
  "safe_caption_claim": "Side-by-side force-directed layouts with shared node positions show that Louvain merges three small ground-truth communities that Leiden resolves separately.",
  "claims_not_supported": [
    "This figure does not report modularity scores.",
    "This figure does not compare runtime or convergence."
  ],
  "panel_row_labels_rendered": true,
  "color_scale_saturation_check": {
    "min_panel_range": 1.0,
    "max_panel_range": 3.5,
    "ratio": 3.5,
    "mitigated_by": "none"
  },
  "panel_layout_actual": {
    "shape": "1 row x 2 columns",
    "rows": [],
    "cols": [
      {"col_id": 1, "col_label": "Louvain"},
      {"col_id": 2, "col_label": "Leiden"}
    ],
    "color_scale_handling": "linear"
  }
}
```

## Conformance check points

| Stage | Check | Mechanism |
|---|---|---|
| 1.5 evidence gate | `addresses_visual_question` matches `figures_planned[].visual_question` verbatim | `academic-pipeline/PIPELINE.md` Stage 1.5 evidence gate |
| 2 (visualization_review_design) | `chart_type_actual` matches `chart_type_chosen` from `notes/figure_design_specs.md`; mismatch without `chart_type_deviation` is FAIL | `visualization_agent.md` Mode 2a algorithm Step 2.1 |
| 5 finalization | `chart_type_actual` in `TRIVIAL_CHART_TYPES` or `NON_TRIVIAL_CHART_TYPES`; not all-trivial without `figure_diversity_exempt` | `scripts/check_chart_type_shortcut.py` |
| 5 finalization | Caption and Results prose stay within `safe_caption_claim`; statements contradicting `claims_not_supported` or asserting `not_shown` variables are flagged | `scripts/check_figure_caption_prose_alignment.py` mechanical alignment pass |
