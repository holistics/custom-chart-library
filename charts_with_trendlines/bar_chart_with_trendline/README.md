# Bar chart with Trendline

## Final Result

![bar-with-trendline](https://github.com/holistics/custom-chart-library/assets/106363759/2cad544d-034e-4c93-aca3-4fb07b516119)

## Full code example

```javascript
CustomChart {
  fields {
    field x {
      type: 'dimension'
      data_type: 'number'
      label: "X axis"
    }
    field y {
      type: 'measure'
      data_type: 'number'
      label: "Y axis"
      }
  }

  options {
    option regression_method {
      label: 'Regression method'
      type: 'select'
      options: ['linear', 'log', 'exp', 'pow', 'quad', 'poly']// chose the regression method
      default_value: 'linear'
    }
    option tooltip {
      type: 'toggle'
      label: 'Show tooltip'
      default_value: true
    }
    option bar_color {
      type: 'color-picker'
      label: 'Bar color'
      default_value: 'blue'
    }
  }

  template: @vgl {
    "data": {
      "values": @{values}
    },
    "layer": [
    {
      "mark": {
      "type": "bar",
        "width": 30,
          "tooltip": @{options.tooltip.value},
          "color": @{options.bar_color.value}
      },
      "encoding": {
        "x": {
          "field": @{fields.x.name},
          "type": "quantitative",
          "axis": {
            "labelAngle": -45,
            "format": @{fields.x.format},
            "formatType": "holisticsFormat"
          }
        },
        "y": {
          "field": @{fields.y.name},
            "type": "quantitative",
            "axis": {
            "format": @{fields.y.format},
            "formatType": "holisticsFormat"
            }
        }
      }
    },
    {
      "mark": {
        "type": "line",
        "color": "firebrick"
      },
      "transform": [
      {
        "regression": @{fields.y.name},
        "on": @{fields.x.name},
        "method": @{options.regression_method.value}
      }
      ],
      "encoding": {
        "x": {
          "field": @{fields.x.name},
          "type": "quantitative"
        },
        "y": {
          "field": @{fields.y.name},
          "type": "quantitative"
        }
      }
    },
    {
      "transform": [
      {
        "regression": @{fields.y.name},
        "on": @{fields.x.name},
        "params": true,
        "method": @{options.regression_method.value}
      },
      {"calculate": "'R²: '+format(datum.rSquared, '.2f')", "as": "R2"} // calculate R-squared
      ],
      "mark": {
        "type": "text",
        "color": "firebrick",
        "fontWeight": "bold",
        "x": "width",
        "align": "center",
        "y": -8
      },
      "encoding": {"text": {"type": "nominal", "field": "R2"}}
    }
    ]
  };;

}
```
