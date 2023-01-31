# Waterfall Chart


<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190190372-9c885f38-4428-4264-8df6-2a8fd77b5c24.png">

## Full code example

```javascript
CustomChart {
  fields {
    // INSERT FIELD DEFINITION HERE
    field amount {
      type: 'measure'
    }
    field label {
      type: 'dimension'
    }
  }
  options {
    // INSERT OPTION DEFINITION HERE
  }
  // INSERT VEGA-LITE JSON SPECIFICATIONS BELOW
  template: @vgl {
    "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
    "data": {
      "values": @{values}
    },
    "width": 800,
    "height": 450,
    "transform": [
      {
        "calculate": "datum['@{fields.amount.name}']",
        "as": "amount"
      },
      {
        "calculate": "datum['@{fields.label.name}']",
        "as": "label"
      },
      {"window": [{"op": "sum", "field": "amount", "as": "sum"}]},
      {"window": [{"op": "lead", "field": "label", "as": "lead"}]},
      {
        "calculate": "datum.lead === null ? datum.label : datum.lead",
        "as": "lead"
      },
      {
        "calculate": "datum.label === 'End' ? 0 : datum.sum - datum.amount",
        "as": "previous_sum"
      },
      {
        "calculate": "datum.label === 'End' ? datum.sum : datum.amount",
        "as": "amount"
      },
      {
        "calculate": "(datum.label !== 'Begin' && datum.label !== 'End' && datum.amount > 0 ? '+' : '') + datum.amount",
        "as": "text_amount"
      },
      {"calculate": "(datum.sum + datum.previous_sum) / 2", "as": "center"},
      {
        "calculate": "datum.sum < datum.previous_sum ? datum.sum : ''",
        "as": "sum_dec"
      },
      {
        "calculate": "datum.sum > datum.previous_sum ? datum.sum : ''",
        "as": "sum_inc"
      }
    ],
    "encoding": {
      "x": {
        "field": "label",
        "type": "ordinal",
        "sort": null,
        "axis": {"labelAngle": 0, "title": "Months"}
      }
    },
    "layer": [
      {
        "mark": {"type": "bar", "size": 45},
        "encoding": {
          "y": {
            "field": "previous_sum",
            "type": "quantitative",
            "title": "Amount"
          },
          "y2": {"field": "sum"},
          "color": {
            "condition": [
              {
                "test": "datum.label === 'Begin' || datum.label === 'End'",
                "value": "#f7e0b6"
              },
              {"test": "datum.sum < datum.previous_sum", "value": "#f78a64"}
            ],
            "value": "#93c4aa"
          }
        }
      },
      {
        "mark": {
          "type": "rule",
          "color": "#404040",
          "opacity": 1,
          "strokeWidth": 2,
          "xOffset": -22.5,
          "x2Offset": 22.5
        },
        "encoding": {
          "x2": {"field": "lead"},
          "y": {"field": "sum", "type": "quantitative"}
        }
      },
      {
        "mark": {"type": "text", "dy": -4, "baseline": "bottom"},
        "encoding": {
          "y": {"field": "sum_inc", "type": "quantitative"},
          "text": {"field": "sum_inc", "type": "nominal"}
        }
      },
      {
        "mark": {"type": "text", "dy": 4, "baseline": "top"},
        "encoding": {
          "y": {"field": "sum_dec", "type": "quantitative"},
          "text": {"field": "sum_dec", "type": "nominal"}
        }
      },
      {
        "mark": {"type": "text", "fontWeight": "bold", "baseline": "middle"},
        "encoding": {
          "y": {"field": "center", "type": "quantitative"},
          "text": {"field": "text_amount", "type": "nominal"},
          "color": {
            "condition": [
              {
                "test": "datum.label === 'Begin' || datum.label === 'End'",
                "value": "#725a30"
              }
            ],
            "value": "white"
          }
        }
      }
    ],
    "config": {"text": {"fontWeight": "bold", "color": "#404040"}}
  };;
}
```

### Ref

[https://vega.github.io/editor/#/examples/vega-lite/waterfall_chart](https://vega.github.io/editor/#/examples/vega-lite/waterfall_chart)
