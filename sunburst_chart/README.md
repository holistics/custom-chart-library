# Sunburst Chart

A sunburst chart visualizes hierarchical composition as concentric rings. The inner ring breaks the total into categories, and outer rings split each category into its children. The wider the arc, the larger the value. Two variants are available: two-level (category + subcategory) and three-level (category + subcategory + item).

- **Good for:** part-to-whole analysis across 2-3 levels of hierarchy (revenue by product line and product, tickets by team and type, budget by department and cost center).
- **Not great for:** flat (non-hierarchical) data, time series, or more than ~10 categories on the inner ring (slices get too thin to read).

![](https://media.holistics.io/86d279dc-sunburst-chart.png)

## Required fields

The inner ring is `level_1`; each additional level radiates outward. A two-level chart takes three fields; the three-level variant takes four. Sort order on the dimension fields sets the angular order of slices within each ring.

**Two-level variant:**

| Field     | Label   | Type        | Role |
|-----------|---------|-------------|------|
| `level_1` | Level 1 | `dimension` | Inner ring (categories). Sorted ascending (`apply_order: 1`). |
| `level_2` | Level 2 | `dimension` | Outer ring (subcategories). Sorted ascending (`apply_order: 2`). |
| `value`   | Value   | `measure`   | Slice size. Sorted descending (`apply_order: 3`). |

**Three-level variant:**

| Field     | Label   | Type        | Role |
|-----------|---------|-------------|------|
| `level_1` | Level 1 | `dimension` | Inner ring (categories). Sorted ascending (`apply_order: 1`). |
| `level_2` | Level 2 | `dimension` | Middle ring (subcategories). Sorted ascending (`apply_order: 2`). |
| `level_3` | Level 3 | `dimension` | Outer ring (items). Sorted ascending (`apply_order: 3`). |
| `value`   | Value   | `measure`   | Slice size. Sorted descending (`apply_order: 4`). |

**Data requirements:** The template aggregates duplicate combinations and filters out null and non-positive values, so you don't need to pre-aggregate. Use positive values, since the template drops zero and negative rows before rendering.

## Options

Both variants share the same options.

| Option         | Default     | Effect |
|----------------|-------------|--------|
| `donut_hole`   | `0.35`      | Center hole radius as a fraction of the total radius. `0` renders a full pie; `0.5` is a large donut. |
| `show_tooltip` | `true`      | Toggles hover tooltips showing value and share of total for each arc. |

## Known limitations

- **Two and three levels only.** The provided templates handle 2-3 rings. Readability degrades past that, and deeper hierarchies need extra data sources and arc marks added to the template.
- **Arc angles are hard to compare precisely.** Sunburst shows composition well but not fine-grained size comparison. Use a bar or treemap chart when exact comparison matters.
- **Color follows Level 1.** Every ring inherits its top-level category color, so you cannot encode a second field through color without editing the template.

## Syntax reference

### As-code syntax
- [sunburst_chart.chart.aml](as-code/sunburst_chart.chart.aml) (two-level)
- [sunburst_chart_three_level.chart.aml](as-code/sunburst_chart_three_level.chart.aml) (three-level)

### Legacy syntax
Not available.
