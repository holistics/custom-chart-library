# Error Bar

An error bar visualizes the uncertainty or degree of error in a reported measurement.

- **Good for:** showing the spread or confidence around a mean per category, comparing variability across groups, summarizing repeated measurements.
- **Not great for:** a single value with no spread (use a KPI or gauge), trends over time, or part-to-whole composition.

![](https://media.holistics.io/40eb724b-error-bar.png)

## Required fields

An Error Bar expects exactly two fields. Each input row is one observation, and the template groups observations by category to compute the mean and spread.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `value`     | Value     | `dimension` | Numeric observation on the x axis (`data_type: 'number'`). The mean sets the point, and the chosen extent sets the bar. Sorted ascending (`apply_order: 1`). |
| `dimension` | Dimension | `dimension` | Category on the y axis (one error bar per value). Sorted ascending (`apply_order: 2`). |

**Data requirements:** Do not pre-aggregate. The template computes the mean and the error extent from the raw rows, so each category needs multiple observations for a meaningful spread.

## Options

| Option   | Default | Effect |
|----------|---------|--------|
| `extent` | `ci`    | How the template computes the error bar length. Options: `ci` (95% confidence interval), `stderr` (standard error), `stdev` (standard deviation), `iqr` (interquartile range). |

## Known limitations

- **Needs multiple rows per category.** The bar reflects spread across observations, so a single row per category produces a point with no visible error bar.
- **The center point is always the mean of `value`.** Showing a median or another statistic requires editing the template.
- **One dimension only.** The chart plots one category axis. Comparing a second grouping (for example, by color) requires editing the template.

## Sample data

💡 Import this sample data into Holistics to use: [barley.csv](https://github.com/holistics/custom-chart-library/files/9571807/barley.csv)

## Syntax reference

### As-code syntax
- [error_bar.chart.aml](as-code/error_bar.chart.aml)

### Legacy syntax
- [error_bar.vgl.aml](legacy/error_bar.vgl.aml)
