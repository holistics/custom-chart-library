# Density Contour Plot

A density contour plot shows where observations concentrate across two numeric metrics, drawing a set of nested contour lines per group like a topographic map of each group's density.

- **Good for:** seeing where groups cluster across two numeric metrics, spotting overlap or separation between segments, replacing an overplotted scatter cloud (spend vs frequency by segment, price vs rating by category).
- **Not great for:** a single ungrouped distribution (use a histogram), comparing one numeric value across categories (use a ridgeline chart), or categorical (non-numeric) axes.

![](https://media.holistics.io/42c139b5-density-contour-chart.png)

## Required fields

A Density Contour Plot expects exactly three fields. Each row of input is one observation, plotted as a point and fed into its group's density estimate.

| Field    | Label  | Type        | Role |
|----------|--------|-------------|------|
| `x_axis` | X-axis | `dimension` | Numeric value on the horizontal axis. Sorted ascending (`apply_order: 1`). |
| `y_axis` | Y-axis | `dimension` | Numeric value on the vertical axis. Sorted ascending (`apply_order: 2`). |
| `group`  | Group  | `dimension` | Splits observations into groups; one contour set and color per group. Sorted ascending (`apply_order: 3`). |

**Data requirements:** Feed raw observations (one row per record), not pre-aggregated values, since the template estimates each group's density with KDE. Both `x_axis` and `y_axis` must be numeric; the template drops rows where either is null before rendering.

## Options

| Option           | Default     | Effect |
|------------------|-------------|--------|
| `bandwidth`      | `0`         | KDE smoothing bandwidth. `0` lets Vega pick automatically; larger values produce smoother, broader contours. |
| `contour_levels` | `4`         | Number of nested contour lines drawn per group. |
| `show_points`    | `true`      | Whether the underlying scatter points are visible. |
| `show_heatmap`   | `false`     | Whether to draw a filled density heatmap behind the contours. |
| `show_tooltip`   | `true`      | Turns the hover tooltip on scatter points and contours on or off. |

## Known limitations

- **Both axes must be numeric.** `x_axis` and `y_axis` feed a 2D KDE, so categorical axes will not work. Use a different chart for non-numeric axes.
- **Needs raw rows, not aggregates.** The density estimate runs on individual observations, so pre-aggregated input gives a misleading shape. Feed one row per record.
- **Sparse groups produce unstable contours.** A group with very few observations yields jumpy or empty contours, since KDE needs enough points to estimate density.

## Syntax reference

### As-code syntax
- [density_contour_plot.chart.aml](as-code/density_contour_plot.chart.aml)

### Legacy syntax
Not available.
