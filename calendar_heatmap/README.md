# Preview

<img width="948" alt="image" src="https://media.holistics.io/5b261de8-calendar-heatmap.png">


# Code

```javascript
CustomChart {
  // ---------------- FIELDS ----------------
  fields {
    field date {
      type: "dimension"
      label: "Date"
      data_type: "date"
    }
    field metric {
      type: "measure"
      label: "Value"
    }
  }

  // ---------------- OPTIONS ----------------
  options {
    option seqScheme {
      type: "select"
      label: "Color scheme (sequential)"
      default_value: "greens"
      options: ["greens","blues","purples","oranges","reds","tealblues","tealgreens","yellowgreens","yelloworangered"]
    }
    option cellSize {
      type: "number-input"
      label: "Cell size (px)"
      default_value: 14
    }
    option scaleType {
      type: "select"
      label: "Scale type"
      default_value: "linear"
      options: ["linear","sqrt","log"]
    }
    // Min / Mid / Max: empty = Auto; any number (pos/neg) = custom
    option domainMin {
      type: "input"
      label: "Color domain min (empty = Auto)"
      default_value: ""
    }
    option domainMid {
      type: "input"
      label: "Color domain mid (empty = Auto)"
      default_value: ""
    }
    option domainMax {
      type: "input"
      label: "Color domain max (empty = Auto)"
      default_value: ""
    }
  }

  // ---------------- TEMPLATE ----------------
  template: @vl {
    "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
    "description": "Calendar heatmap by daily Value with auto-cap (Q3 + 1.5×IQR), Min/Mid/Max overrides (empty = Auto). Legend pins ticks to bottom/middle/top; middle label shows custom Mid if provided.",

    "data": { "values": @{values} },

    "transform": [
      { "timeUnit": "yearmonthdate", "field": "@{fields.date.name}", "as": "day" },
      { "aggregate": [{ "op": "sum", "field": "@{fields.metric.name}", "as": "value_raw" }], "groupby": ["day"] },

      // Auto cap = Q3 + 1.5 * IQR
      { "joinaggregate": [
          { "op": "q1", "field": "value_raw", "as": "q1" },
          { "op": "q3", "field": "value_raw", "as": "q3" }
        ] },
      { "calculate": "datum.q3 - datum.q1", "as": "iqr" },
      { "calculate": "datum.q3 + 1.5 * datum.iqr", "as": "cap_auto" },
      { "calculate": "min(datum.value_raw, datum.cap_auto)", "as": "value_capped" },

      // Auto domain after capping
      { "joinaggregate": [
          { "op": "min", "field": "value_capped", "as": "auto_min" },
          { "op": "max", "field": "value_capped", "as": "auto_max" }
        ] },

      // Effective Min/Max (empty => auto)
      { "calculate": "('@{options.domainMin.value}' !== '' ? toNumber('@{options.domainMin.value}') : datum.auto_min)", "as": "eff_min" },
      { "calculate": "('@{options.domainMax.value}' !== '' ? toNumber('@{options.domainMax.value}') : datum.auto_max)", "as": "eff_max" },

      // Safety
      { "calculate": "datum.eff_min >= datum.eff_max ? datum.eff_max - 1e-6 : datum.eff_min", "as": "eff_min_safe" },
      { "calculate": "datum.eff_max", "as": "eff_max_safe" },

      // Clamp values so the inferred domain is [eff_min, eff_max]
      { "calculate": "max(min(datum.value_capped, datum.eff_max_safe), datum.eff_min_safe)", "as": "value_used" },

      // Year facet
      { "calculate": "year(datum.day)", "as": "year" }
    ],

    "facet": { "row": { "field": "year", "type": "ordinal", "sort": "descending" } },

    "spec": {
      "width": { "step": @{options.cellSize.value} },
      "height": { "step": @{options.cellSize.value} },

      "mark": { "type": "rect", "cornerRadius": 0, "stroke": "#ffffff", "strokeWidth": 0.75 },

      "encoding": {
        "x": { "timeUnit": "week", "field": "day", "type": "ordinal", "title": null, "scale": { "paddingInner": 0, "paddingOuter": 0 } },
        "y": { "timeUnit": "day", "field": "day", "type": "ordinal", "sort": "ascending", "title": null, "scale": { "paddingInner": 0, "paddingOuter": 0 } },

        "color": {
          "field": "value_used",
          "type": "quantitative",
          "scale": {
            "scheme": "@{options.seqScheme.value}",
            "type": "@{options.scaleType.value}",
            "zero": false,
            "nice": { "expr": "('@{options.domainMin.value}' === '' && '@{options.domainMax.value}' === '') ? true : false" },
            "clamp": true
          },
          "legend": {
            "type": "gradient",
            "direction": "vertical",
            "title": "@{fields.metric.name}",
            // Ticks at bottom/middle/top of the color domain
            "values": { "signal": "[domain('color')[0], (domain('color')[0] + domain('color')[1]) / 2, domain('color')[1]]" },
            "tickCount": 3,
            "labelOverlap": "parity",
            // Middle label uses your custom Mid (position stays centered)
            "labelExpr": "datum.value === (domain('color')[0] + domain('color')[1]) / 2 ? ('@{options.domainMid.value}' !== '' ? format(toNumber('@{options.domainMid.value}'), '.3~s') : format(datum.value, '.3~s')) : format(datum.value, '.3~s')",
            "gradientLength": 160,
            "gradientThickness": 12
          }
        },

        "opacity": { "condition": { "test": "isValid(datum.value_used)", "value": 1 }, "value": 0 },

        "tooltip": [
          { "field": "day", "type": "temporal", "title": "Date", "format": "%a %b %d, %Y" },
          { "field": "value_raw", "type": "quantitative", "title": "@{fields.metric.name}" }        ]
      }
    },

    "config": {
      "background": "#ffffff",
      "view": { "stroke": null },
      "axis": { "grid": false, "labelFontSize": 11, "domainColor": "#d1d5db", "tickColor": "#d1d5db" },
      "legend": { "titleFontSize": 12, "labelFontSize": 11, "symbolType": "square" }
    }
  };;
}
```
