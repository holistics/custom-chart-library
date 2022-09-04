# Enable Cross Filter for all sub-charts in a Mixed Chart

## Goal

When a dashboard cross filter is generated, it would be applied for all sub-charts in the chart.

https://user-images.githubusercontent.com/27631976/188302677-b4c9b073-ba3f-4921-8447-f746b7b308b7.MP4

# Prerequisite: 

For instructions on how to build a mixed chart, see: https://github.com/holistics/custom-chart-library/blob/main/Advanced Custom Charts/Mixed Chart type - Bar Chart with Average Line.md.

```javascript

CustomChart {
    fields {
      field x_axis {
        type: 'dimension'
      }
      field legend {
        type: 'dimension'
        data_type: 'string'
      }
      field y_axis {
        type: 'measure'
      }
    }
    options {
    }
    template: @vgl {
      "data": {"values": @{values}},
      "layer": [{
        "params": [{
          "name": "intervalSelection",
          "select": {"type": "interval", "encodings": ["x"]}
        }],
        "mark": {
          "type": "bar",
          "tooltip": true
        },
        "encoding": {
          "x": {
            "field": @{fields.x.name},
            "axis": {
              "format": @{fields.x.format},
              "formatType": "holisticsFormat",
              "labelAngle": 45
            }
          },
          "y": {
            "field": @{fields.y.name},
            "type": "quantitative",
            "axis": {
              "format": @{fields.y.format},
              "formatType": "holisticsFormat"
            }
          },
          "xOffset": {
            "field": @{fields.legend.name}
          },
          "color": {
            "field": @{fields.legend.name}
          },
          "opacity": {
            "condition": {
              "param": "intervalSelection", "value": 1
            },
            "value": 0.7
          }
        }
      }, {
        "transform": [{
          "filter": {"param": "intervalSelection"}
        }],
        "mark": "rule",
        "encoding": {
          "y": {
            "aggregate": "mean",
            "field": @{fields.y.name},
            "type": "quantitative"
          },
          "color": {"value": "firebrick"},
          "size": {"value": 3}
        }
      }],
      "holisticsConfig": { 
        "crossFilterSignals": ["intervalSelection"], 
        "contextMenuSignals": [] 
      }
    };;
  }
```
