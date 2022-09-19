# Linear Regression R^2 

Final Result

<img width="929" alt="image" src="https://user-images.githubusercontent.com/27631976/190141947-41632f40-7ce7-43a0-886a-14183e2c17ed.png">

```javascript
CustomChart {
  fields {
    field a {
      type: "dimension"
      label: "First Quantitative Variable"
    },

    field b {
      type: "dimension"
      label: "Second Quantitative Variable"
    }
  }
  
  options {
    // INSERT OPTION DEFINITION HERE
    option num_type {
      label: "Type of Quantitaive Variable"
      type: "select"
      options: ["discrete", "continuous"]
      default_value: "discrete"
    },

    option cat_type {
      label: "Type of Qualitative Variable"
      type: "select"
      options: ["nominal", "ordinal"]
      default_value: "nominal"
    }
  }
  // INSERT VEGA-LITE JSON SPECIFICATIONS BELOW
  template: @vgl {
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
            "field": @{fields.a.name},
            "type": "quantitative"
          },
          "y": {
            "field": @{fields.b.name},
            "type": "quantitative"
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
            "regression": @{fields.b.name},
            "on": @{fields.a.name}
          }
        ],
        "encoding": {
          "x": {
            "field": @{fields.a.name},
            "type": "quantitative"
          },
          "y": {
            "field": @{fields.b.name},
            "type": "quantitative"
          }
        }
      },
      {
        "transform": [
          {
            "regression": @{fields.b.name},
            "on": @{fields.a.name},
            "params": true
          },
          {"calculate": "'R²: '+format(datum.rSquared, '.2f')", "as": "R2"}
        ],
        "mark": {
          "type": "text",
          "color": "firebrick",
          "x": "width",
          "align": "right",
          "y": -5
        },
        "encoding": {
          "text": {"type": @{options.cat_type.value}, "field": "R2"}
        }
      }
    ]

    

  };;
}
```
