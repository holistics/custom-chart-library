# Chord Diagram

A chord diagram visualizes flows and their magnitude between a set of categories. Each category is an arc around a circle, and curved bands (chords) connect them: the arc size reflects a category's total volume, and a chord's width reflects the strength of connection between two categories.

- **Good for:** many-to-many relationships between categories, movement or flow between groups, source-target pairs with values (country-to-country trade, customer movement between subscription plans).
- **Not great for:** one-way funnels or journeys (use a Sankey Chart), hierarchical part-to-whole data (use a Treemap or Sunburst Chart), or more than ~15 categories around the ring.

<img width="2028" height="1297" alt="Chord Diagram" src="https://media.holistics.io/7465db80-chord-diagram.png" />

## Required fields

A Chord Diagram expects exactly three fields. Each row of input is one directed connection from a source category to a target category.

| Field    | Label  | Type        | Role |
|----------|--------|-------------|------|
| `source` | Source | `dimension` | Originating category of the connection. Sorted ascending (`apply_order: 1`). |
| `target` | Target | `dimension` | Destination category of the connection. Sorted ascending (`apply_order: 2`). |
| `value`  | Value  | `measure`   | Connection strength; sets the chord width and contributes to each arc's size. Sorted descending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per source-target pair (for example, `SUM(value)` grouped by `source` and `target`); the `chord` transform does not combine duplicate pairs. Categories that appear as both source and target share a single arc on the ring.

## Options

| Option               | Default  | Effect |
|----------------------|----------|--------|
| `pad_angle`          | `0.05`   | Gap in radians between each arc segment around the ring. |
| `inner_radius_ratio` | `0.9`    | Fraction of the outer radius that the ribbon inner edge fills. |
| `label_padding`      | `80`     | Pixels reserved outside the arc for category labels. |
| `show_tooltip`       | `true`   | Shows a tooltip with category or flow details on hover. |

## Known limitations

- **Pre-aggregate first.** The `chord` transform does not sum duplicate source-target pairs, so repeated rows skew arc and chord sizes. Aggregate to one row per pair before charting.
- **Readability drops past ~15 categories.** Beyond roughly 15 categories around the ring the chords overlap and labels collide. Group small categories into an "Other" bucket first.

## Sample data

💡 Sankey sample data can be reused: [sankey-sample-data.csv](https://github.com/holistics/custom-chart-library/files/11058203/sankey-sample-data.csv)

## Syntax reference

### As-code syntax
- [chord_diagram.chart.aml](as-code/chord_diagram.chart.aml)

### Legacy syntax
- [chord_diagram.vgl.aml](legacy/chord_diagram.vgl.aml)
