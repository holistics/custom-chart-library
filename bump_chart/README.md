# Bump Chart

A bump chart tracks how categories rank against each other over time, highlighting overtakes and trend reversals.

- **Good for:** rank-over-time stories like top sellers by month, leaderboard movement, or competitive standings where overtakes matter more than raw values.
- **Not great for:** comparing exact values (use a line chart), a single before-and-after comparison (use the Slope Chart), or more than ~10 categories where lines crowd together.

![](https://media.holistics.io/7461ed58-bump-chart.png)

## Required fields

A Bump Chart expects exactly three fields. Each row of input is one category's value in one period, and the template ranks categories within each period.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `period`    | Period    | `dimension` | Position on the x-axis; one column of points per period. Sorted ascending (`apply_order: 1`). |
| `dimension` | Dimension | `dimension` | The category whose rank the chart tracks, with one line per category. Sorted ascending (`apply_order: 2`). |
| `value`     | Value     | `measure`   | The amount used to rank categories within each period (highest value ranks first). Sorted descending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per period and category, since the template ranks rows directly and does not sum duplicates. Provide a value for every category in every period so each line is continuous, and use at least two periods so ranks have something to move between.

## Options

| Option            | Default     | Effect |
|-------------------|-------------|--------|
| `line_interpolate`| `monotone`  | Line shape between points. `monotone` draws smooth curves; `linear` draws straight segments. |
| `point_size`      | `100`       | Size of the point markers at each period. |
| `show_tooltip`    | `true`      | Shows a tooltip with category, period, rank, and value on hover. |

## Known limitations

- **Needs one value per category per period.** A category missing in a period breaks its line and shifts ranks for that column, so fill gaps before charting.
- **Shows rank, not magnitude.** The y-axis is rank order, so equal gaps between ranks can hide large value differences. Use a line chart when the actual values matter.
- **Readability drops past ~10 categories.** Beyond roughly 10 lines the ranks crowd and crossings get hard to follow. Group smaller categories together first.

## Syntax reference

### As-code syntax
- [bump_chart.chart.aml](as-code/bump_chart.chart.aml)

### Legacy syntax
Not available.
