# Waterfall Chart

A waterfall chart visualizes how intermediate values contribute to a total, particularly the cumulative effect of sequential positive or negative changes. Common applications include financial statements, P&L analysis, budget variances, and sales funnels.

- **Good for:** profit-and-loss breakdowns, budget variance, revenue bridges, any running total built from sequential positive and negative steps.
- **Not great for:** part-to-whole composition without an order (use a treemap or sunburst), independent category comparison (use a bar chart), or time series with one value per period.

<img alt="reporting-waterfall-chart-thumbnail" title="reporting-waterfall-chart-thumbnail" src="https://cdn.holistics.io/product/reporting-waterfall-chart-thumbnail-20250224-655.png" width="1326" height="746" />

## Required fields

A Waterfall Chart expects exactly three fields. Each row of input is one step in the sequence.

| Field        | Label      | Type        | Role |
|--------------|------------|-------------|------|
| `amount`     | Amount     | `measure`   | Signed change for the step. Positive values step up, negative values step down. Sorted descending (`apply_order: 1`). |
| `label`      | Label      | `dimension` | Step name shown on the x-axis. Sorted ascending (`apply_order: 2`). |
| `sort_order` | Sort Order | `dimension` | Numeric position that sets the left-to-right order of steps. Sorted ascending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per step; the template computes the running total in order of `sort_order`, so every step needs a distinct sort value. Use signed amounts (positive for increases, negative for decreases). The template appends a final total bar automatically, so you don't add a total row yourself.

## Options

| Option            | Default   | Effect |
|-------------------|-----------|--------|
| `begin_label`     | (empty)   | Label of the step treated as the starting bar, colored with the begin/end color. |
| `end_label`       | `Total`   | Label used for the auto-generated final total bar. |
| `bar_padding`     | `0.3`     | Inner spacing between bars, from 0 to 0.9. Higher values make thinner bars. |
| `begin_end_color` | `#BDBDBD` | Fill color for the begin and end (total) bars. |
| `begin_end_text`  | `black`   | Text color for the labels on the begin and end bars. |
| `positive`        | `#58A65C` | Fill color for steps that increase the running total. |
| `negative`        | `#D85140` | Fill color for steps that decrease the running total. |
| `value_format`    | `$,.0f`   | Number format string applied to amounts and totals (d3-format syntax). |

## Known limitations

- **Sort order drives everything.** The template accumulates the running total in `sort_order` sequence, so missing or duplicate sort values produce a wrong or jumbled total. Give each step a unique, gap-free order.
- **The chart generates the total bar; don't add your own.** It appends a final bar named by `end_label`, so including your own total row double-counts it.
- **Label text identifies the begin and end bars.** A step becomes the start or total only when its `label` exactly matches `begin_label` or `end_label`. Mismatched text leaves those bars colored as ordinary steps.

## Syntax reference

### As-code syntax
- [waterfall_chart.chart.aml](as-code/waterfall_chart.chart.aml)

### Legacy syntax
- [waterfall_chart.vgl.aml](legacy/waterfall_chart.vgl.aml)
