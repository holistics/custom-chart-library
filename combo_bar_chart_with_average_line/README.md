# Bar Chart with Average Line

A bar chart with an average line overlays a horizontal mean line across a grouped bar chart, so viewers can instantly see which bars sit above or below the overall average.

- **Good for:** spotting over- and under-performers against the series average, comparing categories grouped by a second dimension, benchmark-style comparisons.
- **Not great for:** time-based moving averages (use Bar Chart with Running Average Line), part-to-whole composition, or a single bar with no comparison group.

![](https://media.holistics.io/fd47d802-bar-chart-with-average-line.png)

## Required fields

A Bar Chart with Average Line expects exactly three fields. Each row is one bar, grouped along the x-axis by `dimension` and split into side-by-side bars by `series`.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | X-axis category; one group of bars per value. Sorted ascending (`apply_order: 1`). |
| `series`    | Series    | `dimension` | Splits each group into side-by-side bars and drives the color and legend. Sorted ascending (`apply_order: 2`). |
| `value`     | Value     | `measure`   | Bar height. The average line is the mean of this field across all rows. Sorted descending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per `dimension` and `series` pair; the bar layer plots values as-is and does not combine duplicates. The average line is the mean of every plotted `value`, so it reflects all bars in view.

## Known limitations

- **One flat average across the whole series.** The line is a single mean over all plotted rows, not a per-group or per-series average. For a trend that moves period by period, use Bar Chart with Running Average Line instead.
- **Requires a series field.** The definition has three fields, including `series` for the color split. With only one category per x value, leave `series` constant or pick a simple bar chart.
- **Average is sensitive to outliers.** A few very large or very small bars pull the mean line away from the typical value, which can mislead at a glance.

## Syntax reference

### As-code syntax
- [bar_chart_with_average_line.chart.aml](as-code/bar_chart_with_average_line.chart.aml)

### Legacy syntax
- [combo_bar_chart_with_average_line.vgl.aml](legacy/combo_bar_chart_with_average_line.vgl.aml)
