# Slope Chart

A slope chart compares each category's value at two points in time, with one line per category whose slope tells the story. The chart colors lines by direction (up, down, flat) and labels both ends, so there is no legend to decode.

- **Good for:** before-and-after comparisons like this quarter vs last quarter revenue by region, NPS by segment around a launch, or cost per team across two budget cycles.
- **Not great for:** trends across many periods (use the Bump Chart for ranks or a line chart for values), or more than ~10 categories where labels and lines crowd together (use the Faceted Sparkline).

![](https://media.holistics.io/0d9c7893-slope-chart.png)

## Required fields

A Slope Chart expects exactly three fields. Each row is one category's value in one period, and the template draws one line per category between the first and last period.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `period`    | Period    | `dimension` | The two endpoints on the x-axis; the template uses only the first and last period values. Sorted ascending (`apply_order: 1`). |
| `dimension` | Dimension | `dimension` | The category; one slope line per category, labeled at both ends. Sorted ascending (`apply_order: 2`). |
| `value`     | Value     | `measure`   | The amount plotted at each endpoint, setting line height and slope direction. Sorted ascending (`apply_order: 3`). |

**Data requirements:** The template sums duplicate category rows within a period and drops null values, so you don't need to pre-aggregate. Each category needs a value at both the first and last period; the chart drops categories present in only one of the two. If the data has more than two periods, it compares only the first and last.

## Options

| Option           | Default     | Effect |
|------------------|-------------|--------|
| `increase_color` | `#2cb67f`   | Color for lines that go up between the two periods. |
| `decrease_color` | `#e5484d`   | Color for lines that go down between the two periods. |
| `show_change`    | `true`      | When on, appends the percent change to each category's right-hand (end) label. |
| `show_tooltip`   | `true`      | Toggles the hover tooltip showing category, start value, end value, and change. |

## Known limitations

- **Compares exactly two periods.** With more than two periods, the chart plots only the first and last and ignores the middle. Use the Bump Chart or a line chart to show every period.
- **Categories need both endpoints.** The chart drops any category missing at either the first or last period, since it has no slope to draw.
- **Readability drops past ~10 categories.** Beyond roughly 10 lines the direct labels overlap and slopes get hard to separate. Group smaller categories together first.

## Syntax reference

### As-code syntax
- [slope_chart.chart.aml](as-code/slope_chart.chart.aml)

### Legacy syntax
Not available.
