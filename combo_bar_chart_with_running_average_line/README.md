# Bar Chart with Running Average Line

A bar chart with a running average line overlays a moving-average line on a time-based bar chart, so you can read each period's value against a smoothed trend in one view.

- **Good for:** smoothing noisy time series, spotting trend direction under volatile bars, comparing each period to its recent neighbors.
- **Not great for:** non-time categories, comparing against a single flat benchmark (use Bar Chart with Average Line), or part-to-whole composition.

<img alt="Bar chart with a running average line tracking the trend across bars" title="Bar Chart with Running Average Line" src="https://media.holistics.io/1c4e0e68-bar-chart-with-running-average-line.png" width="900" height="600" />

## Required fields

A Bar Chart with Running Average Line expects exactly two fields. Each row is one bar, ordered along a time axis.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Time axis (plotted as `temporal`); sets bar order and the running-average window. Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `measure`   | Bar height and the input to the running average. Sorted descending (`apply_order: 2`). |

**Data requirements:** `dimension` must be a date or datetime, since the x-axis is `temporal`. Pre-aggregate to one row per time period; the template sorts by `dimension` and computes the running average over a sliding window but does not combine duplicate periods.

## Options

| Option           | Default   | Effect |
|------------------|-----------|--------|
| `tooltip`        | `true`    | Toggles hover tooltips on the bars and the average line's points. |
| `line_color`     | `#E5484D` | Color of the running average line and its points. |
| `points_before`  | `-3`      | Window start, in rows before the current point, for the running average. `-3` averages the current period and the three before it. |
| `points_after`   | `0`       | Window end, in rows after the current point. `0` stops the window at the current period. |

## Known limitations

- **Time axis only.** The x-axis is `temporal`, so `dimension` must be a date or datetime. Non-time categories will not plot correctly.
- **Window is row-based, not calendar-based.** `points_before` and `points_after` count rows, so gaps in the time series (missing periods) skew the average. Fill missing periods first for an even smoothing window.
- **Early bars have a partial window.** The first few points average fewer rows than the full window, so the start of the line is less smoothed than the rest.

## Syntax reference

### As-code syntax
- [bar_chart_with_running_average_line.chart.aml](as-code/bar_chart_with_running_average_line.chart.aml)

### Legacy syntax
- [bar-chart-with-moving-average.vgl.aml](legacy/bar-chart-with-moving-average.vgl.aml)
