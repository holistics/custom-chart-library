# Packed Bubble Chart

A packed bubble chart shows category magnitudes as circles sized by value, where bubbles gravitate toward the center with a physics simulation and settle into place. Labels sit inside the larger bubbles, the layout adapts continuously to the container, and cluster tightness is tunable via the gravity options. Hovering a bubble highlights it and fades the others.

- **Good for:** comparing magnitudes across a single set of categories, showing relative size at a glance (revenue by product, headcount by team, traffic by source).
- **Not great for:** precise value comparison (use a bar chart), part-to-whole hierarchy (use a sunburst or treemap), or data that needs x/y coordinates (use a bubble plot).

<img alt="reporting-custom-chart/packed_bubble" title="reporting-custom-chart/packed_bubble" src="https://cdn.holistics.io/product/reporting-custom-chart/packed_bubble-20241101-363.png" width="900" height="600" />

## Required fields

A Packed Bubble Chart expects exactly two fields. Each row of input is one category whose value sets the bubble size.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category each bubble represents; shown as the bubble label. Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `measure`   | Magnitude that sets the bubble area. Sorted descending (`apply_order: 2`). |

**Data requirements:** The template sums duplicate categories and filters out null and non-positive values, so you don't need to pre-aggregate. Use positive values, since the template drops zero and negative rows before rendering.

## Options

| Option         | Default | Effect |
|----------------|---------|--------|
| `gravity_x`    | `0.1`   | Horizontal pull toward the center. Higher values pack bubbles tighter left-to-right. |
| `gravity_y`    | `0.2`   | Vertical pull toward the center. Higher values pack bubbles tighter top-to-bottom. |
| `show_labels`  | `true`  | Whether to show text labels inside bubbles large enough to fit them. |
| `show_tooltip` | `true`  | Toggles the hover tooltip on each bubble. |

## Known limitations

- **Positions are not quantitative.** Bubbles settle by physics simulation, so their x/y location carries no meaning. Use a bubble plot when you need two numeric axes.
- **Labels show only on larger bubbles.** Small bubbles omit their label even with `show_labels` on, since the text would not fit inside the circle.
- **Magnitudes are hard to compare precisely.** Area-based sizing reads relative scale well but not exact differences. Use a bar chart when precise comparison matters.

## Syntax reference

### As-code syntax
- [packed_bubble_force.chart.aml](as-code/packed_bubble_force.chart.aml)

### Legacy syntax
- [packed_bubble.vg.aml](legacy/packed_bubble.vg.aml)
