# Sankey Chart

A Sankey chart visualizes how a subject moves between states or categories. Each node represents a state, and each link represents the volume flowing from one state to another. The wider the link, the larger the flow.

- **Good for:** user journeys, funnel analysis, budget or resource allocation.
- **Not great for:** cyclic data, time series, or charts with more than ~30 unique nodes.

<img alt="reporting-custom-chart/sankey" title="reporting-custom-chart/sankey" src="https://media.holistics.io/4a9b0332-sankey-chart.png" width="900" height="600" />

## Required fields

A Sankey Chart expects exactly three fields. Each row of input is one directed link from a source node to a target node.

| Field    | Label  | Type        | Role |
|----------|--------|-------------|------|
| `source` | Source | `dimension` | Originating node of the link. Sorted ascending (`apply_order: 1`). |
| `target` | Target | `dimension` | Destination node of the link. Sorted ascending (`apply_order: 2`). |
| `value`  | Value  | `measure`   | Flow volume; sets the link width. Sorted descending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per source-target pair (for example, `SUM(value)` grouped by `source` and `target`); the template does not combine duplicate links. Use non-negative values, since zero-value rows render as invisible links. A node may appear in both `source` and `target`, where it renders as a pass-through node.

## Options

| Option           | Default    | Effect |
|------------------|------------|--------|
| `show_tooltip`   | `true`     | Toggles the hover tooltip on flow links showing source, target, and value. |
| `node_align`     | `justify`  | How nodes are positioned horizontally: `justify` spreads them across all columns, `left`/`right`/`center` aligns them to one side. |
| `node_width`     | `15`       | Width of each node rectangle in pixels. |
| `node_padding`   | `10`       | Vertical spacing between nodes in the same column. |
| `margin_top`     | `10`       | Top padding between the chart edge and the diagram. |
| `margin_left`    | `10`       | Left padding between the chart edge and the diagram. |
| `margin_right`   | `10`       | Right padding between the chart edge and the diagram. |
| `margin_bottom`  | `10`       | Bottom padding between the chart edge and the diagram. |

## Known limitations

- **No cyclic flows.** The source-to-target graph must be acyclic. Circular paths cause a Vega runtime error, so reshape or remove cycles before charting.
- **Readability drops past ~30 nodes.** Beyond roughly 30 unique nodes the links get too thin to distinguish. Aggregate small nodes into an "Other" group first.

## Sample data

💡 Import this data into Holistics to use: [sankey-sample-data.csv](https://github.com/holistics/custom-chart-library/files/11058203/sankey-sample-data.csv)

## Syntax reference

### As-code syntax
- [sankey_chart.chart.aml](as-code/sankey_chart.chart.aml)

### Legacy syntax
- [sankey_chart.vgl.aml](legacy/sankey_chart.vgl.aml)
- [sankey-sample.vg.json](legacy/sankey-sample.vg.json) (compiled Vega output, kept for reference)
- [sankey-sample-data.json](legacy/sankey-sample-data.json) (original sample dataset)
