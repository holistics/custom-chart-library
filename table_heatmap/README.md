# Table Heatmap

A table heatmap colors each cell of a calendar grid by a measure value, mapping day of month across the x axis and month down the y axis. It is useful for spotting daily and seasonal patterns at a glance, where darker cells mean larger values.

- **Good for:** daily activity over a year (orders per day, logins per day), seasonal patterns, spotting unusually high or low days.
- **Not great for:** non-date dimensions, comparing exact values between cells, or data spanning more than one calendar year (months collapse together).

<img alt="reporting-custom-chart/table_heatmap" title="reporting-custom-chart/table_heatmap" src="https://media.holistics.io/5581e3d4-table-heatmap.png" width="900" height="600" />

## Required fields

A Table Heatmap expects exactly two fields. Each row supplies one date and its measured value, and the template buckets those dates into a day-by-month grid.

| Field   | Label | Type        | Role |
|---------|-------|-------------|------|
| `date`  | Date  | `dimension` | Date field; its day of month sets the x position and its month sets the y position. Sorted ascending (`apply_order: 1`). |
| `value` | Value | `measure`   | Drives the cell color, from light (low) to dark (high). Sorted descending (`apply_order: 2`). |

**Data requirements:** The `date` field must be a date type. The template takes `max` of `value` per day-month cell, so you don't need to pre-aggregate, though one row per day keeps the values exact.

## Known limitations

- **Date dimension only.** The x and y positions come from the day and month of one date field, so this chart cannot map two arbitrary dimensions.
- **One calendar year at a time.** The y axis uses month with no year component, so dates from different years stack onto the same row. Filter to a single year to keep cells distinct.
- **Cells show one aggregated value.** Each cell renders the `max` value for that day-month, so multiple rows on the same date collapse into a single colored cell rather than separate marks.

## Sample data

💡 Import this data into Holistics to use: [seattle-weather.csv](https://github.com/holistics/custom-chart-library/files/9571836/seattle-weather.csv)

## Syntax reference

### As-code syntax
- [table_heatmap.chart.aml](as-code/table_heatmap.chart.aml)

### Legacy syntax
- [table_heatmap.vgl.aml](legacy/table_heatmap.vgl.aml)
