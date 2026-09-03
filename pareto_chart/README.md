# Pareto Chart

A Pareto chart combines sorted bars with a cumulative percentage line to show which categories contribute most of the total. It answers 80/20 questions like "which customers drive most of our revenue?" at a glance.

- **Good for:** 80/20 analysis, ranking categories by contribution, finding the vital few that drive most of a total (revenue by customer, defects by cause, sales by product).
- **Not great for:** time series, part-to-whole composition across a hierarchy (use a sunburst or treemap), or data with too many categories to label along the x-axis.

![](https://media.holistics.io/7e02370f-pareto-chart.png)

## Required fields

A Pareto Chart expects exactly two fields. Each row is one category with its value.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category shown as a bar along the x-axis. Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `measure`   | Bar height; also drives the descending sort and the cumulative percentage line. Sorted descending (`apply_order: 2`). |

**Data requirements:** Pre-aggregate to one row per category, since the template does not combine duplicate categories before sorting and accumulating. The template sorts categories by `value` in descending order and computes each one's cumulative share of the grand total, so values should be non-negative for the cumulative line to climb correctly.

## Options

| Option           | Default   | Effect |
|------------------|-----------|--------|
| `line_color`     | `#e5484d` | Color of the cumulative percentage line and its point markers. |
| `show_tooltip`   | `true`    | Toggles hover tooltips on bars and the cumulative line. |
| `show_threshold` | `true`    | Shows or hides the horizontal threshold reference line. |
| `threshold`      | `0.8`     | Threshold value (0 to 1) for the reference line. The label shows the formatted percentage. |

## Known limitations

- **The chart sorts categories by value, not by your dimension order.** The template forces a descending sort on `value`, so you cannot keep a custom category order on the x-axis.
- **Cumulative line assumes non-negative values.** The template computes the cumulative percentage against the grand total, so negative values distort the climb and can push the line outside the 0 to 1 range.
- **Too many categories crowd the axis.** Every category gets a labeled bar, so large category counts make the x-axis hard to read; group small categories into an "Other" bucket first.

## Syntax reference

### As-code syntax
- [pareto_chart.chart.aml](as-code/pareto_chart.chart.aml)

### Legacy syntax
Not available.
