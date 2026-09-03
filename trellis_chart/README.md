# Trellis Chart

A Trellis plot (or small multiple) is a series of similar plots that displays different subsets of the same data, facilitating comparison across subsets.

- **Good for:** comparing the same bar breakdown across segments (sales by category per region, counts by stage per team), spotting which segments differ from the rest, laying out many small bar charts in a stacked grid of rows.
- **Not great for:** time-series x-axes (the template treats the x-axis as ordinal and may not handle dates well), a single segment (a plain bar chart is simpler), or part-to-whole composition (use a sunburst or treemap chart).

![](https://media.holistics.io/f571ada3-trellis-chart.png)

## Required fields

A Trellis Chart expects exactly three fields. `facet` becomes one row of bars per value; within each row, `x_axis` and `value` draw the bars.

| Field    | Label  | Type        | Role |
|----------|--------|-------------|------|
| `facet`  | Facet  | `dimension` | Splits the data into one row (small multiple) per value. Sorted ascending (`apply_order: 1`). |
| `x_axis` | X-axis | `dimension` | Categorical x position within each row. Sorted ascending (`apply_order: 2`). |
| `value`  | Value  | `measure`   | Bar height (y). Sorted descending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per facet and x-axis combination; the template plots `value` directly as bars without summing. The template treats the x-axis as ordinal, so use categorical values rather than dates. The template also computes a per-row mean of `value` (shown in the tooltip as "Mean for row").

## Options

| Option              | Default      | Effect |
|---------------------|--------------|--------|
| `show_y_axis_label` | `true`       | Toggles the y-axis labels in each row. |
| `row_sort`          | `ascending`  | Orders the facet rows ascending or descending. |
| `row_height`        | `'25'`       | Height of each row's plot, in pixels. |
| `row_width`         | `'800'`      | Width of the chart, in pixels. |
| `row_space`         | `'25'`       | Vertical gap between rows, in pixels. |

## Known limitations

- **X-axis is ordinal, not temporal.** The template treats dates on the x-axis as categories, so a true time axis may not render correctly. Use categorical x-values, or another chart for time series.
- **Width and height are fixed values, not container-fit.** `row_width` and `row_height` set static pixel sizes, so wide content can overflow or leave whitespace. Tune the size options to your layout.
- **Many facets make a tall chart.** Each facet value adds a row, so a large number of facets produces a long scroll. Filter to the segments you want to compare.

## Syntax reference

### As-code syntax
- [trellis_chart.chart.aml](as-code/trellis_chart.chart.aml)

### Legacy syntax
- [trellis_chart.vgl.aml](legacy/trellis_chart.vgl.aml)
