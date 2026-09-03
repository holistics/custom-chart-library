# Annotated Time Series

A line chart with labeled event markers, so viewers can connect metric movements to campaigns, releases, and incidents.

- **Good for:** annotating a single metric's trend with releases, campaigns, or incidents; explaining a spike or dip by the event that caused it; sharing a timeline where context matters as much as the numbers.
- **Not great for:** comparing many series at once (use a multi-line or faceted sparkline chart), categorical data with no date axis, or trends that have no notable events to mark.

![](https://media.holistics.io/beca8cdb-annotated-time-series.png)

## Required fields

An Annotated Time Series expects exactly three fields. The template draws the line from `date` and `value`; `event_label` adds the dashed markers and labels.

| Field         | Label       | Type        | Role |
|---------------|-------------|-------------|------|
| `date`        | Date        | `dimension` | Time axis (x). Sorted ascending (`apply_order: 1`). |
| `value`       | Value       | `measure`   | Line height (y). Sorted descending (`apply_order: 2`). |
| `event_label` | Event Label | `dimension` | Event name shown as a vertical marker. Sorted ascending (`apply_order: 3`). |

**Data requirements:** Pre-aggregate to one row per date; the template plots `value` directly without summing, so duplicate dates draw a jagged or overlapping line. Populate `event_label` only on the dates that have an event and leave it null or empty otherwise (the template skips rows where it is null or `''` for markers but still draws them on the line).

## Options

| Option        | Default     | Effect |
|---------------|-------------|--------|
| `event_color` | `#e5484d`   | Color of the event marker rules and their labels. |
| `show_tooltip`| `true`      | Toggles hover tooltips on event markers and data points. |

## Known limitations

- **Event markers depend on a sparse label column.** The label field must be null or empty on non-event dates. A value on every row draws a marker on every date and clutters the chart.
- **No second metric.** The template plots one `value` series. Comparing several metrics needs a different chart or a template edit.
- **Dense event labels overlap.** Labels render vertically at each event date, so many events close together collide. Keep events spaced or filter to the notable ones.

## Syntax reference

### As-code syntax
- [annotated_time_series.chart.aml](as-code/annotated_time_series.chart.aml)

### Legacy syntax
Not available.
