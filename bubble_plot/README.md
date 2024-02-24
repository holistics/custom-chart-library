# Bubble Plot

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190325049-3943d728-57c0-4b7b-a9cf-31839105b862.png">


# Sample Data

💡 Import this data into Holistics to use: [disasters.csv](https://github.com/holistics/custom-chart-library/files/9572233/disasters.csv)


# Code

```javascript
CustomChart {
  fields {
    field x_axis {
      type: "dimension"
      label: "X axis"
    }

    field y_axis {
      type: "dimension"
      label: "Y axis"
    }

    field size {
      type: "dimension"
      label: "Size"
    }

  }

  template: @vgl {
    "data": {
      "values": @{values}
    },
    "mark": {
      "type": "circle",
      "opacity": 0.8,
      "stroke": "black",
      "strokeWidth": 1
    },
    "encoding": {
      "x": {
        "field": @{fields.x_axis.name},
        "type": "temporal",
        "axis": {"grid": false}
      },
      "y": {
        "field": @{fields.y_axis.name},
        "type": "nominal",
        "axis": {"title": ""}
      },
      "size": {
        "field": @{fields.size.name},
        "type": "quantitative",
        "title": "Annual Global Deaths",
        "legend": {"clipHeight": 30},
        "scale": {"rangeMax": 5000}
      },
      "color": {"field": @{fields.y_axis.name}, "type": "nominal", "legend": null}
    }
  };;
}
```
