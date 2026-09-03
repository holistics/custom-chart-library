# Treemap

A treemap displays hierarchical data as a set of nested rectangles. Each branch of the tree is a rectangle whose area is proportional to a value in the data.

- **Good for:** part-to-whole composition across one category (revenue by product, storage by folder, headcount by team), comparing many categories by size in a compact space.
- **Not great for:** precise value comparison (use a bar chart), time series, flow between categories (use a Sankey Chart or Chord Diagram), or categories with negative values.

![](https://media.holistics.io/609f2dfa-treemap.png)

## Required fields

A Treemap expects exactly two fields. Each row of input becomes one rectangle.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category each rectangle represents; also drives the rectangle label. Sorted ascending (`apply_order: 1`). |
| `value`     | Value     | `measure`   | Rectangle area, the percentage label, and the fill opacity (relative to the largest value). Sorted descending (`apply_order: 2`). |

**Data requirements:** Pre-aggregate to one row per category (for example, `SUM(value)` grouped by `dimension`); the template sizes a rectangle per row and does not combine duplicate categories. The template keeps only rows where `value` is non-null and greater than zero, so it drops any zero or negative rows before rendering.

## Options

| Option              | Default     | Effect |
|---------------------|-------------|--------|
| `text_color`        | `white`     | Color of the category label and percentage text inside each rectangle. |
| `text_length_limit` | `200`       | Maximum width of the category label, in pixels. The template truncates longer labels. |
| `show_tooltip`      | `true`      | Toggles the hover tooltip showing dimension name, value, and percentage of total. |
| `layout_method`     | `squarify`  | Treemap tiling algorithm that controls rectangle shapes and arrangement. Options: `binary`, `squarify`, `dice`, `resquarify`, `slice`, `slicedice`. |

## Known limitations

- **Pre-aggregate first.** The template draws one rectangle per row and does not sum duplicate categories, so repeated category rows render as separate overlapping rectangles. Aggregate to one row per category first.
- **The template drops non-positive values.** It filters out rows where `value` is null, zero, or negative, so categories with those values do not appear at all.
- **Single level only.** The template renders a flat set of rectangles from one dimension; it does not nest sub-categories. Use a Sunburst Chart for multi-level hierarchies.

## Syntax reference

### As-code syntax
- [treemap.chart.aml](as-code/treemap.chart.aml)

### Legacy syntax
- [treemap.vg.aml](legacy/treemap.vg.aml)
