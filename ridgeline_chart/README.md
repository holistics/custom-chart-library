# Ridgeline Chart

A ridgeline chart compares the distribution of a numeric value across categories, drawing one smooth density curve per category in a compact stack. Where a box plot shows summary statistics, a ridgeline shows the actual shape (skew, peaks, outlier tails) of each group.

- **Good for:** comparing the shape of one numeric value across categories, spotting skew or multiple peaks per group, seeing how distributions shift (delivery time by carrier, order value by segment).
- **Not great for:** a single ungrouped distribution (use a histogram), two-metric density (use a density contour plot), or exact summary statistics (use a box plot).

![](https://media.holistics.io/bd9412d7-ridgeline-chart.png)

## Required fields

A Ridgeline Chart expects exactly two fields. Each row of input is one observation; the chart groups rows by `dimension` and estimates a density curve per group.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category that gets one ridge (density curve). Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `dimension` | Numeric value whose distribution each ridge shows. Sorted ascending (`apply_order: 2`). |

**Data requirements:** Feed raw observations (one row per record), not pre-aggregated values, since the template estimates each category's density with KDE. `value` must be numeric; the template drops rows where it is null before rendering.

## Options

| Option            | Default     | Effect |
|-------------------|-------------|--------|
| `overlap`         | `2`         | How much each ridge overlaps the one above it. Higher values stack the curves more tightly. |
| `bandwidth`       | `0`         | KDE smoothing bandwidth. `0` lets Vega pick automatically; larger values produce smoother curves. |
| `scale_by_count`  | `false`     | When on, ridge height reflects each category's record count instead of scaling every ridge to its own shape. |
| `show_tooltip`   | `true`  | Toggles the category label tooltip on hover over each ridge. |

## Known limitations

- **`value` must be numeric.** The KDE runs on `value`, so a non-numeric field will not produce a density curve.
- **Needs raw rows, not aggregates.** The density estimate runs on individual observations, so pre-aggregated input gives a misleading shape. Feed one row per record.
- **The chart clips outliers to the Tukey fences.** It clamps the visible range to 1.5 IQR beyond the quartiles, so values in the extreme tails fall outside the drawn range.

## Syntax reference

### As-code syntax
- [ridgeline_chart.chart.aml](as-code/ridgeline_chart.chart.aml)

### Legacy syntax
Not available.
