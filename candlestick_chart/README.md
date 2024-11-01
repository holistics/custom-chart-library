# Candlestick/OHLC Chart

## Goal

From this dataset:
![illustration](https://user-images.githubusercontent.com/27631976/188304837-bcec6b16-f358-471f-b8a6-760115438953.png)

We will define a Candlestick/OHLC Custom Chart that looks like this:
![illustration](https://github.com/holistics/custom-chart-library/assets/106363759/44ef7d51-f366-434d-b1bf-a8b753746994)

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
      label: "Date field"
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

  	options {
		option tooltip {
			type: 'toggle'
			label: 'Show tooltip'
			default_value: true
		}
		option green_candle {
			type: 'color-picker'
			label: 'Green candlestick'
			default_value: 'green'
		}
    option red_candle {
			type: 'color-picker'
			label: 'Red candlestick'
			default_value: 'red'
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
          "y": {"field": @{fields.low.name}},
          "y2": {"field": @{fields.high.name}}
        }
      },
      {
        "mark": {
          "type": "bar",
          "tooltip": @{options.tooltip.value}
        },
        "encoding": {
          "y": {"field": @{fields.open.name}},
          "y2": {"field": @{fields.close.name}}
        }
      }
    ],
    "encoding": {
      "x": {
        "axis": {
          "format": "%m/%d",
          "labelAngle": -45
        },
        "type": "temporal",
        "field": @{fields.date.name},
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
      "color": {
        "condition": {
          "test": "datum.@{fields.open.name} < datum.@{fields.close.name}",
          "value": @{options.green_candle.value}
        },
        "value": @{options.red_candle.value}
      }
    }
  };;
}

```
