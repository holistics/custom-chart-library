# Control Chart (XmR)

A control chart (XmR, also called a process behaviour chart) tracks a metric over time and separates real change from routine noise. It draws the metric with its natural process limits, plus a companion Moving Range panel showing point-to-point volatility.

- **Good for:** monitoring a metric over time for real change (weekly signups, defect counts, delivery times, support volume), spotting outliers and trends with statistical limits.
- **Not great for:** comparing categories side by side, part-to-whole composition, or data without a natural time or sequence order (use a bar or line chart instead).

![](https://media.holistics.io/d7134102-control-chart.png)

## Required fields

A Control Chart expects exactly two fields. Each row is one observation in the time series.

| Field    | Label  | Type        | Role |
|----------|--------|-------------|------|
| `period` | Period | `dimension` | Time or sequence axis; one point per period. Sorted ascending (`apply_order: 1`). |
| `value`  | Value  | `measure`   | Metric plotted as individual values, with limits derived from its moving range. Sorted ascending (`apply_order: 2`). |

**Data requirements:** Pre-aggregate to one row per period, since the template does not combine duplicate periods; it sorts by `period` and reads each point in order, so the periods must form a clean ordered sequence. The template drops null values before computing the limits. Limits stabilize with more points (the long-run rule needs at least eight consecutive points), so very short series produce weak signals.

## Options

| Option           | Default     | Effect |
|------------------|-------------|--------|
| `show_mr_chart`  | `true`      | Shows or hides the companion Moving Range panel below the main chart. |
| `show_tooltip`   | `true`      | Turns the hover tooltip on both panels on or off. |

## Known limitations

- **Needs an ordered time series.** The template sorts by `period` and reads points in sequence, so the data must have one row per period in a meaningful order. Flat or unordered category data does not produce valid limits.
- **Signals are weak on short series.** Limits come from the average moving range, and the long-run rule needs at least eight consecutive points, so very short series detect little.
- **One metric at a time.** The chart plots a single value field against a single period field; it cannot overlay or compare multiple series.

## Syntax reference

### As-code syntax
- [control_chart.chart.aml](as-code/control_chart.chart.aml)

### Legacy syntax
Not available.
