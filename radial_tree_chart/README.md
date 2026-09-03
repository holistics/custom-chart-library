# Radial Tree

A radial tree draws hierarchical data as a node-link diagram fanning out from a central root, with depth shown by distance from the center.

- **Good for:** category taxonomies (subcategory under category under department), org structures, or any nested grouping shown as a branching shape across 1-3 levels.
- **Not great for:** part-to-whole sizing (use a Treemap or Sunburst Chart, which size nodes by value), flat (non-hierarchical) data, or deep trees with more than three levels.

![](https://media.holistics.io/460cce26-radial-tree-chart.png)

## Required fields

A Radial Tree expects up to three fields, all dimensions. `level_1` is required; `level_2` and `level_3` are optional, so a branch can stop early when a deeper level is null. Each row contributes one path from the root down through its non-null levels.

| Field     | Label   | Type        | Role |
|-----------|---------|-------------|------|
| `level_1` | Level 1 | `dimension` | First branch under the root (required). Sorted ascending (`apply_order: 1`). |
| `level_2` | Level 2 | `dimension` | Second branch under Level 1 (optional). Sorted ascending (`apply_order: 2`). |
| `level_3` | Level 3 | `dimension` | Outermost branch under Level 2 (optional). Sorted ascending (`apply_order: 3`). |

**Data requirements:** No measure is needed; the template builds the tree from the level columns alone, and node positions do not depend on any value. The template drops rows with a null or empty `level_1` and collapses duplicate node paths, so you don't need to pre-aggregate. Each child should roll up to a single parent across the columns, otherwise the same label appears under multiple branches.

## Options

| Option         | Default | Effect |
|----------------|---------|--------|
| `root_label`   | `All`   | Text shown on the central root node that all Level 1 branches connect to. |
| `layout`       | `tidy`  | Tree layout algorithm. `tidy` spaces nodes by structure; `cluster` aligns all leaf nodes at the same outer radius. |
| `link_shape`   | `curve` | Style of the connecting links between nodes. |
| `spread`       | `360`   | Angular span the tree fans across, in degrees. `360` is a full circle; lower values open a wedge. |
| `show_labels`  | `true`  | Toggles the text label next to each node. |
| `show_tooltip` | `true`  | Toggles the hover tooltip showing the label, depth, and child count for each node. |

## Known limitations

- **Three levels maximum.** The template reads only `level_1` through `level_3`. Deeper hierarchies need extra level columns and matching node logic added to the template.
- **Nodes are not sized by value.** Every node is the same size and color encodes depth, not magnitude. Use a Treemap or Sunburst Chart when arc or area should reflect a measure.
- **Each child must roll up to one parent.** A label that appears under more than one parent renders as separate nodes, which misrepresents the hierarchy. Make sure the level columns form a clean tree.

## Syntax reference

### As-code syntax
- [radial_tree.chart.aml](as-code/radial_tree.chart.aml)

### Legacy syntax
Not available.
