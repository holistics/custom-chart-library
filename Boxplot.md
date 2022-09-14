# Simple Boxplot

Final Result
![](image.png)


Final Result (exploration with tooltip; showing summaries)
![](image.png)


```javascript
CustomChart {
    fields {
        field a {                    // this is to define to holistics the first field input
            type: "dimension"
            label: "Categorical field"
        }

        field b {                  // this is to define to holistics the second field input
            type: "dimension"            
            label: "Quantitative field"
        }
    }

    options {
        option tooltip {
            type: 'toggle'
            label: 'Show tooltip'
            default_value: true
        }
    }

  template: @vgl {
    "data": {
            "values": @{values}
    },

    "mark": {
      "type": "boxplot",
      "extent": "min-max",
      "tooltip": @{options.tooltip.value}
    },

    "encoding": {
      "x": {
          "field": @{fields.a.name}, 
          "type": "nominal"
      },

      "color": {
          "field": @{fields.a.name}, 
          "type": "nominal", 
          "legend": null
      },
      
      "y": {
        "field": @{fields.b.name},
        "type": "quantitative",
        "scale": {"zero": false}
      }
    }
    

  };;
}
```
