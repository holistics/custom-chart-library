# Enable Cross Filter on user’s point selection


> **What is Cross Filtering?**

> Cross filtering creates shared filters that are applied to all report widgets in your dashboard. To learn more about this feature, head over to: https://docs.holistics.io/docs/cross-filtering#what-is-cross-filtering. 


## Outcome

When a user selects a data range on your chart, a cross filter containing the selected range would be applied to all report widgets in your dashboard. 


https://user-images.githubusercontent.com/27631976/188298824-d8772e71-8345-412a-9d74-d383f2f1ea23.MP4


## Prerequisite: 

For highlight/stroke effect on user’s point selection, please see [Create highlight/stroke effect on user’s point selection guide](https://github.com/holistics/custom-chart-library/blob/main/Interactive%20Custom%20Charts/Highlight-Stroke%20Effect%20on%20Point%20Selection.md).


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
      "holisticsConfig": {
        "crossFilterSignals": ["normalPointSelection"],
      }
    };;
  }
```



