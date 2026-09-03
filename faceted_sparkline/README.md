# Faceted Sparkline

A faceted sparkline shows the same metric's trend for many categories side by side, one compact panel per category. Because each panel scales independently, you compare the shape of each trend rather than absolute values.

- **Good for:** comparing the shape of a trend across many categories (revenue per region, sign-ups per channel, a KPI across teams), spotting which segments are growing or declining, replacing a multi-line chart that has turned into spaghetti.
- **Not great for:** comparing absolute values across panels (each panel scales independently), a single category (a plain line chart is simpler), or part-to-whole composition (use a sunburst or treemap chart).

![](https://media.holistics.io/9a9abea1-faceted-sparkline.png)

## Required fields

A Faceted Sparkline expects exactly four fields. `facet` makes one panel per category, and within each panel `series` draws one line per series.

| Field    | Label  | Type        | Role |
|----------|--------|-------------|------|
| `date`   | Date   | `dimension` | Time axis (x) within each panel. Sorted ascending (`apply_order: 1`). |
| `value`  | Value  | `measure`   | Line height (y), scaled per panel. Sorted ascending (`apply_order: 2`). |
| `facet`  | Facet  | `dimension` | Splits the data into one panel per value. Sorted ascending (`apply_order: 3`). |
| `series` | Series | `dimension` | Draws one colored line per value within each panel. Sorted ascending (`apply_order: 4`). |

**Data requirements:** Pre-aggregate to one row per date, facet, and series combination; the template plots `value` directly without summing. The template drops rows where `value` is null. If every facet has only one series, supply a constant `series` value so each panel still draws a single line.

## Options

| Option           | Default     | Effect |
|------------------|-------------|--------|
| `show_tooltip`   | `true`      | Toggles the hover tooltip on each line. |
| `facet_columns`  | `4`         | Number of panels per row; the grid wraps to as many rows as needed. |

## Known limitations

- **Panels do not share a y-scale.** Each panel scales independently, so it shows trend shape, not absolute size. Use a single chart with a shared axis when you need to compare magnitudes across categories.
- **Many facets shrink each panel.** Every facet value gets its own panel, so a large number of facets leaves each one too small to read. Filter to the categories you care about or raise `facet_columns`.
- **No dedicated axis labels per panel.** Panels show the trend, a header value, and a hover crosshair, but no full axis ticks. Reach for a regular line chart when exact axis values matter.

## Syntax reference

### As-code syntax
- [faceted_sparkline.chart.aml](as-code/faceted_sparkline.chart.aml)

### Legacy syntax
Not available.
