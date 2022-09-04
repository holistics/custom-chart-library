# Create Candlestick/OHLC Chart

## Goal

From this dataset:

We will define a Candlestick/OHLC Custom Chart that looks like this:


## Mechanism

By using 2 types of marks, `rule` (a straight line) and `bar` from Vega-lite, on a mixed chart (using `layer` property), we are able to create the candlestick shape.

To learn more about Vega-lite marks, see: https://vega.github.io/vega-lite/docs/mark.html#types. 
To learn more about layer property, see: https://vega.github.io/vega-lite/docs/layer.html. 

## Full-code

```javascript

CustomChart {
  fields {
    field date {
      type: "dimension"
      label: "Pick a date field"
    }
    field low {
      type: "dimension"
      label: "Low price"
    }
    field high {
      type: "dimension"
      label: "High price"
    }
    field open {
      type: "dimension"
      label: "Open price"
    }
    field close {
      type: "dimension"
      label: "Close price"
    }
  }
  template: @vgl
  {
    "data": {
      "values": @{values}
    },
		"layer": [
			{
				"mark": "rule",
				"encoding": {
					"y": {
						"field": @{fields.low.name}
					},
					"y2": {
						"field": @{fields.high.name}
					}
				}
			},
			{
				"mark": "bar",
				"encoding": {
					"y": {
						"field": @{fields.open.name}
					},
					"y2": {
						"field": @{fields.close.name}
					}
				}
			}
		],
		"encoding": {
			"x": {
				"axis": {
					"title": "Date in 2009",
					"format": "%m/%d",
					"labelAngle": -45
				},
				"type": "temporal",
				"field": @{fields.date.name},
				"title": "Date in 2009"
			},
			"y": {
				"axis": {
					"title": "Price"
				},
				"type": "quantitative",
				"scale": {
					"zero": false
				}
			},
		}
	};;
}

```
