### Introduction

In this guide, we will guide you on how to add a regression trendline over a scatterplot that models field `"y"` as a function of `"x"` 

![linear regression trend line](https://user-images.githubusercontent.com/27631976/194109892-746c5fb4-74f2-4616-a714-8a3276149611.png)

### Full code example

```json
CustomChart {
  fields {
    field x {
      type: 'dimension'
      data_type: 'number'
    }
    field y {
      type: 'dimension'
      data_type: 'number'
    }
  }
  options {
    option regression_method {
      label: 'Regression method'
      type: 'select'
      options: ['linear', 'log', 'exp', 'pow', 'quad', 'poly']// chose the regression method
      default_value: 'linear'
    }
  }
  template: @vgl {
    "data": {
      "values": @{values}
    },
    "layer": [
      {
        "mark": {
          "type": "point",
          "filled": true,
          "tooltip": true
        },
        "encoding": {
          "x": {
            "field": @{fields.x.name},
            "type": "quantitative",
            "axis": {
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
          "x": "width",
          "align": "center",
          "y": -5
        },
        "encoding": {"text": {"type": "nominal", "field": "R2"}}
      }
    ]
  };;
}
```
