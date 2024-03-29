# Create highlight/stroke effect on point selection

## Outcome: 

When a user selects a column, or a range of columns (using Shift-Left Click), the data should be highlighted. Additionally, on mouse hover, there should be a stroke outlining the column.

https://user-images.githubusercontent.com/27631976/188298593-f929edc9-6f3e-467b-8a19-ae0920fa1c36.MP4


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
          "name": "pointSelectionOnMouseOver",
          "select": {"type": "point", "on": "mouseover"}
        },
        {"name": "normalPointSelection", "select": "point"},
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
          "condition": {"param": "normalPointSelection", "value": 1},
          "value": 0.3
        },
        "strokeWidth": {
          "condition": [
            {
              "param": "normalPointSelection",
              "empty": false,
              "value": 2
            },
            {
              "param": "pointSelectionOnMouseOver",
              "empty": false,
              "value": 1
            }
          ],
          "value": 0
        }
      },
      "config": {
        "scale": {
          "bandPaddingInner": 0.2
        },
      },
    };;
  }
```
