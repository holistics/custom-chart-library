# Histogram

A histogram groups numerical values into discrete ranges (bins) and uses bars to show how many values fall in each bin. It is a simple yet effective way to grasp the distribution of a numeric variable.

- **Good for:** seeing the distribution of a single numeric variable, spotting skew or outliers, checking how values cluster (order value, response time, age).
- **Not great for:** comparing distributions across categories (use a ridgeline chart), categorical counts (use a bar chart), or two-metric density (use a density contour plot).

<img alt="reporting-custom-chart/histogram" title="reporting-custom-chart/histogram" src="https://media.holistics.io/20b2e228-histogram.png" width="900" height="600" />

## Required fields

A Histogram expects exactly two fields. Each row of input is one record; the chart bins the numeric `value` and counts how many records land in each bin.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Grain of the data (one row per record). Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `dimension` | Numeric value to bin along the x-axis. Sorted ascending (`apply_order: 2`). |

**Data requirements:** Feed raw rows (one per record), not pre-aggregated counts, since the template bins `value` and counts records itself. `value` must be numeric.

## Options

| Option         | Default | Effect |
|----------------|---------|--------|
| `max_bins`     | `40`    | Maximum number of bins the histogram will use. Smaller values produce wider, fewer bins; larger values produce narrower, more bins. |
| `show_tooltip` | `true`  | Toggles the hover tooltip on bars. |

## Known limitations

- **`value` must be numeric.** The bin transform runs on `value`, so a non-numeric field cannot be binned. Use a bar chart for categorical counts.
- **Needs raw rows, not aggregates.** The chart counts records per bin, so pre-summarized input distorts the distribution. Feed one row per record.
- **Bin width is approximate.** `max_bins` is an upper bound, not an exact count; Vega rounds to a readable bin width, so the rendered bar count can be lower.

## Syntax reference

### As-code syntax
- [histogram.chart.aml](as-code/histogram.chart.aml)

### Legacy syntax
- [histogram_chart.vgl.aml](legacy/histogram_chart.vgl.aml)
