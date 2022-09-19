# Preview

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190314670-68dc3426-eca0-4fb6-8379-a6a844ed0b4b.png">


# Sample Data

<aside>
💡 Import this sample data into Holistics to use

</aside>

[barley.csv](https://github.com/holistics/custom-chart-library/files/9571807/barley.csv)

# Code

<aside>
💡 Copy and paste this code into More Settings > Custom Chart

</aside>

```javascript
CustomChart {
  fields {
    field point {
      type: "dimension"
      label: "Points"
    }
    field variety {
      type: "dimension"
      label: "Variety"
    }
  }
  template: @vgl
  {
      "data": {
        "values": @{values}
      },
      "layer": [
        {
          "mark": {
            "type": "point",
            "filled": true
          },
          "encoding": {
            "x": {
              "type": "quantitative",
              "field": @{fields.point.name},
              "scale": {
                "zero": false
              },
              "title": "Barley Yield",
              "aggregate": "mean"
            },
            "color": {
              "value": "black"
            }
          }
        },
        {
          "mark": {
            "type": "errorbar",
            "extent": "ci"
          },
          "encoding": {
            "x": {
              "type": "quantitative",
              "field": @{fields.point.name},
              "title": "Barley Yield"
            }
          }
        }
      ],
      "encoding": {
        "y": {
          "type": "ordinal",
          "field": @{fields.variety.name}
        }
      }
    }
  ;;;
}
```
