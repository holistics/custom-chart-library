# Diverging Bar Chart

A diverging bar chart splits positive and negative values around a zero baseline, ideal for variance against budget or plan. The chart sorts categories by value, so the biggest gains and shortfalls sit at opposite ends.

- **Good for:** variance against budget or plan, net sentiment or profit-and-loss by category, any signed measure where direction matters.
- **Not great for:** all-positive measures (a plain bar chart is clearer), part-to-whole composition, or time series.

![](https://media.holistics.io/3ba4bd01-diverging-bar-chart.png)

## Required fields

A Diverging Bar Chart expects exactly two fields. Each row is one horizontal bar.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category for each bar (y-axis). The template re-sorts bars by value at render time, so the longest bars sit at the ends. Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `measure`   | Bar length and direction. Negative values extend left of the zero baseline, positive values extend right. Sorted descending (`apply_order: 2`). |

**Data requirements:** Pre-aggregate to one row per `dimension`; the template plots values as-is and does not combine duplicates. Use a signed measure with both positive and negative values, otherwise the bars all point the same way and there is nothing to diverge. The template assigns color by sign (`value >= 0` gets the positive color, the rest get the negative color).

## Options

| Option           | Default   | Effect |
|------------------|-----------|--------|
| `positive_color` | `#2cb67f` | Color of bars with a value at or above zero. |
| `negative_color` | `#e5484d` | Color of bars with a value below zero. |
| `show_tooltip`   | `true`    | Toggles hover tooltips on the bars. |

## Known limitations

- **Needs signed values to diverge.** Color and direction are driven by the sign of `value`. With all-positive or all-negative data every bar points the same way, so a plain bar chart reads more clearly.
- **Color carries no other meaning.** The two colors only mark positive versus negative, so you cannot encode a separate series or category through color without editing the template.
- **Bar order follows value, not your sort.** Bars always re-sort by value (largest at the ends), so the field's own sort order does not control vertical placement.

## Syntax reference

### As-code syntax
- [diverging_bar_chart.chart.aml](as-code/diverging_bar_chart.chart.aml)

### Legacy syntax
Not available.
