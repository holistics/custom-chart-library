# Create a Bar Chart with Average Line

### Goal

A Bar Chart with an average line showing the mean of y-axis value.

![](https://user-images.githubusercontent.com/27631976/188299724-76a85d58-2438-4d6f-9dcf-6278fe5badba.png)

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
        }
      }, {
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
    };;
  }
```

