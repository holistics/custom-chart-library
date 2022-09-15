# Preview

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190325049-3943d728-57c0-4b7b-a9cf-31839105b862.png">


# Sample Data

<aside>
💡 Import this data into Holistics to use

</aside>
[disasters.csv](https://github.com/holistics/custom-chart-library/files/9572233/disasters.csv)


# Code

```javascript
CustomChart {
  fields {
    field x_axis {
      type: "dimension"
      label: "Categorical"
    }

    field y_axis {
      type: "dimension"
      label: "Numerical"
    }
  }

  template: @vgl {
    "data": {
      "values": @{values}
    },
    "mark": {
      "type": "boxplot",
      "extent": "min-max",
      "size": 50
    },
    "encoding": {
      "x": {
        "field": @{fields.x_axis.name}, 
        "type": "nominal",
        "axis": {
          "labelAngle": 0
        }
        },
      
      "color": {"field": @{fields.x_axis.name}, "type": "nominal", "legend": null},
      "y": {
        "field": @{fields.y_axis.name},
        "type": "quantitative",
        "scale": {"zero": false}
      }
   } 
  };;

}
```
