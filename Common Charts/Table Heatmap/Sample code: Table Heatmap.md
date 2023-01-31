# Preview

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190315469-98de3c55-5d3e-4e2e-9803-6ec59ea7dfa4.png">


# Sample Data

💡 Import this data into Holistics to use: [seattle-weather.csv](https://github.com/holistics/custom-chart-library/files/9571836/seattle-weather.csv)


# Code

```javascript
CustomChart {
  fields {
    field date {
      type: 'dimension',
      label: "Pick a date field",
    }
    field temperature {
      type: 'measure',
      label: "Then a number field",
    }
  }
  template: @vgl {
    "data": {
      "values": @{values}
    },
    "mark": {
      "type": "rect",
      "tooltip": true
    },
    "config": {
      "axis": {
        "domain": false
      },
      "view": {
        "step": 13,
        "strokeWidth": 0
      }
    },
    "encoding": {
      "x": {
        "axis": {
          "format": "%e",
          "labelAngle": 0
        },
        "type": "ordinal",
        "field": @{fields.date.name},
        "title": "Day",
        "timeUnit": "date"
      },
      "y": {
        "type": "ordinal",
        "field": @{fields.date.name},
        "title": "Month",
        "timeUnit": "month"
      },
      "color": {
        "type": "quantitative",
        "field": @{fields.temperature.name},
        "legend": {
          "title": null
        },
        "aggregate": "max"
      }
    }
  }
  ;; 
}
```
