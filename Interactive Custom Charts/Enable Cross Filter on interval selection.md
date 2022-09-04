# Enable Cross Filter on user’s interval selection

> **What is Cross Filtering?**

> Cross filtering creates shared filters that are applied to all report widgets in your dashboard. To learn more about this feature, head over to: https://docs.holistics.io/docs/cross-filtering#what-is-cross-filtering. 


## Prerequisite: 

For highlight/stroke effect on user’s selection, please see [Create highlight/stroke effect on user’s point selection guide](https://github.com/holistics/custom-chart-library/blob/main/Interactive%20Custom%20Charts/Highlight-Stroke%20Effect%20on%20Point%20Selection.md).


## Outcome: 

When a user creates an interval selection (by holding Left Click then dragging it across your chosen values) on your chart, a cross filter would be applied to all report widgets in your dashboard. 


```javascript
CustomChart {
    fields {
      field x_axis {
        type: 'dimension'
      }
      field measure {
        type: 'measure'
      }
    }
    options {
      option color {
        type: 'color-picker'
        default_value: '#484848'
      }
    }
    template: @vgl {
      "data": {
        "values": @{values}
      },
      "params": [
        {
          "name": "intervalSelection",
          "select": {"type": "interval", "encodings": ["x"]} }
      ],
      "mark": {
        "type": "bar",
        "fill": @{options.color.value},
        "cursor": "pointer",
        "stroke": "black",
        "tooltip": true
      },
      "encoding": {
        "x": {
          "field": @{fields.x_axis.name},
          "type": "ordinal",
          "axis": {
            "format": @{fields.x_axis.format},
            "formatType": "holisticsFormat",
            "labelAngle": 45
          }
        },
        "y": {
          "field": @{fields.measure.name},
          "type": "quantitative",
          "axis": {
            "format": @{fields.measure.format},
            "formatType": "holisticsFormat"
          }
        },
        "fillOpacity": {
          "condition": {"param": "intervalSelection", "value": 1},
          "value": 0.3
        },
        "strokeWidth": {
          "condition": [
            {
              "param": "intervalSelection",
              "empty": false,
              "value": 1
            },
          ],
          "value": 0
        }
      },
      "config": {
        "scale": {
          "bandPaddingInner": 0.2
        },
      },
      "holisticsConfig": {
        "crossFilterSignals": ["intervalSelection"],
      }
    };;
  }
```
