# Bullet Chart

A bullet chart shows a measure against a target and qualitative ranges, packing KPI-versus-goal context into a compact bar.

- **Good for:** tracking KPIs against a goal, comparing actual versus target versus pace across categories, compact scorecards on a dashboard.
- **Not great for:** showing a trend over time (use a line chart), comparing many measures at once, or a single metric with no target (use a gauge chart).

![](https://media.holistics.io/4a601342-bullet-chart.png)

## Required fields

A Bullet Chart expects exactly four fields. Each row is one category with its target, pace, and current values.

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category on the y axis (one bullet per value). Sorted ascending (`apply_order: 1`). |
| `target`    | Target    | `measure`   | Goal value, drawn as a vertical tick. Sorted descending (`apply_order: 2`). |
| `pace`      | Pace      | `measure`   | Expected-to-date value, drawn as the wide background bar. Sorted descending (`apply_order: 3`). |
| `current`   | Current   | `measure`   | Actual value, drawn as the thin foreground bar. Sorted descending (`apply_order: 4`). |

**Data requirements:** Pre-aggregate to one row per `dimension` value; the template folds the three measures per row but does not combine duplicate categories.

## Options

| Option                  | Default | Effect |
|-------------------------|---------|--------|
| `current_bar_height`    | `8`     | Height in pixels of the thin Current bar. |
| `target_tick_thickness` | `2`     | Thickness in pixels of the Target tick mark. |

## Known limitations

- **All three measures must be on the same scale.** Target, pace, and current share one x axis, so they need comparable units for the bullet to read correctly.
- **One value per category.** The template does not aggregate, so duplicate `dimension` rows draw overlapping marks. Pre-aggregate to a single row per category.
- **Fixed three-series layout.** The chart always draws target, pace, and current. It cannot show additional series or qualitative range bands without editing the template.

## Syntax reference

### As-code syntax
- [bullet_chart.chart.aml](as-code/bullet_chart.chart.aml)

### Legacy syntax
Not available.
