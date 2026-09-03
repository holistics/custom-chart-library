# Gauge Chart

A gauge chart visualizes a metric against a threshold, to monitor progress or benchmark against a target.

- **Good for:** showing a single metric against its maximum, monitoring progress toward a goal, at-a-glance KPI panels with a benchmark.
- **Not great for:** comparing many categories at once (use a bar or bullet chart), trends over time, or part-to-whole composition.

Two variants are available:
- **Simple Gauge Chart**: value against a maximum.
- **Gauge Chart with Target**: adds a target marker to benchmark against a goal.

## Simple Gauge Chart

![Simple Gauge Chart](https://cdn.holistics.io/product/reporting-custom-chart/simple-gauge-chart-20241102-375.png)

### Required fields

| Field       | Label     | Type      | Role |
|-------------|-----------|-----------|------|
| `value`     | Value     | `measure` | Current metric, drawn as the filled arc and needle. Sorted descending (`apply_order: 1`). |
| `max_value` | Max Value | `measure` | Top of the gauge range (the 100% end). Sorted descending (`apply_order: 2`). |

**Data requirements:** Provide a single row. The template reads one record and computes `value / max_value` as the fill percentage, so pre-aggregate to one value per measure. Keep `max_value` greater than zero, since it is the divisor for the gauge percentage.

### Options

| Option         | Default | Effect |
|----------------|---------|--------|
| `needleScale`  | `0.8`   | Needle length as a fraction of the inner radius. |
| `showLabels`   | `true`  | Toggles the 0 and max-value end labels. They also hide on their own when the gauge is too small to show them legibly. |
| `showTicks`    | `true`  | Toggles the evenly spaced tick marks. |
| `tickGaps`     | `0.25`  | Spacing between ticks as a fraction of the range (smaller means more ticks). |
| `show_tooltip` | `true`  | Turns the hover tooltip on or off. |

This variant takes no color options. The filled arc uses your workspace color palette, and the track, needle, ticks and text follow the app theme, so the gauge matches the rest of your dashboard in both light and dark mode.

## Gauge Chart with Target

![Gauge Chart with Target](https://cdn.holistics.io/product/reporting-custom-chart/gauge-chart-with-target-20241102-376.png)

### Required fields

| Field       | Label     | Type      | Role |
|-------------|-----------|-----------|------|
| `value`     | Value     | `measure` | Current metric, drawn as the filled arc and needle. Sorted descending (`apply_order: 1`). |
| `max_value` | Max Value | `measure` | Top of the gauge range (the 100% end). Sorted descending (`apply_order: 2`). |
| `target`    | Target    | `measure` | Goal value, drawn as a colored marker on the arc. Sorted descending (`apply_order: 3`). |

**Data requirements:** Provide a single row. The template reads one record and computes `value / max_value` as the fill percentage, so pre-aggregate to one value per measure. Keep `max_value` greater than zero, since it is the divisor for the gauge percentage.

### Options

| Option         | Default   | Effect |
|----------------|-----------|--------|
| `needleScale`  | `0.8`     | Needle length as a fraction of the inner radius. |
| `tickGaps`     | `0.25`    | Spacing between ticks as a fraction of the range (smaller means more ticks). |
| `showLabels`   | `true`    | Toggles the 0 and max-value end labels. |
| `showTicks`    | `true`    | Toggles the evenly spaced tick marks. |
| `show_tooltip` | `true`    | Turns the hover tooltip on or off. |
| `targetColor`  | `#5fc93c` | Color of the target marker on the arc. |

Aside from the target marker, this variant takes no color options either: the filled arc uses your workspace palette, and the needle, ticks, and text follow the app theme, matching the Simple Gauge Chart.

## Known limitations

- **Single value only.** Each gauge renders one record. To compare categories, use a separate gauge per value or switch to a bar or bullet chart.
- **`max_value` must be greater than zero.** It is the divisor for the fill percentage, so a zero or missing maximum produces an invalid gauge.
- **Half-circle range fixed at 0 to max.** The arc always runs from 0 to `max_value` across a 180-degree sweep. Custom minimums or full-circle gauges require editing the template.

## Syntax reference

### As-code syntax
- [gauge_chart.chart.aml](as-code/gauge_chart.chart.aml) (simple)
- [gauge_chart_with_target.chart.aml](as-code/gauge_chart_with_target.chart.aml) (with target)

### Legacy syntax
- [gauge_chart_simple.vgl.aml](legacy/gauge_chart_simple.vgl.aml) (simple)
- [gauge_chart_with_target.vgl.aml](legacy/gauge_chart_with_target.vgl.aml) (with target)
