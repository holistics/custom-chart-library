### Goal

In this guide, we will create a Relative Frequency Histogram - a type of graph that shows how often something happens in percentages that looks like this:

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190314227-eac80c90-7f0b-4a8c-80c0-551c0c8c51ff.png">

### Full code example

```javascript
//The data is binned with first transform. 
// The number of values per bin and the total number are calculated in the second and third transform 
// Calculate the relative frequency in the last transformation step.

CustomChart {
  fields {
    field x_axis {
      type: "dimension"
      label: "dimension"
    }
  }

  template: @vgl {
    "data": {
      "values": @{values}
    },
    "transform": [{
        "bin": true, "field": @{fields.x_axis.name}, "as": "bin_Horsepwoer"
      }, {
        "aggregate": [{"op": "count", "as": "Count"}],
        "groupby": ["bin_Horsepwoer", "bin_Horsepwoer_end"]
      }, {
        "joinaggregate": [{"op": "sum", "field": "Count", "as": "TotalCount"}]
      }, {
        "calculate": "datum.Count/datum.TotalCount", "as": "PercentOfTotal"
      }
    ],
    "mark": {"type": "bar", "tooltip": true},
    "encoding": {
      "x": {
        "title": @{fields.x_axis.name},
        "field": "bin_Horsepwoer",
        "bin": {"binned": true}
      },
      "x2": {"field": "bin_Horsepwoer_end"},
      "y": {
        "title": "Frequency",
        "field": "PercentOfTotal",
        "type": "quantitative",
        "axis": {
          "format": ".1~%"
          }
      }
    }
  } ;;

}
```
