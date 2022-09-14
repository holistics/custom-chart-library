# Simple Boxplot

Final Result

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190141668-afca099e-f1e2-4fcb-b451-3422f5b9b691.png">


Final Result (exploration with tooltip; showing summaries)

<img width="761" alt="Screenshot 2022-09-14 113833" src="https://user-images.githubusercontent.com/27631976/190141067-ead4bef4-c404-4b8c-a9e5-c36d09696409.png">


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
