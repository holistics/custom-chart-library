# Simple Bar Chart
![](https://vega.github.io/vega-lite/examples/bar.png)

```javascript
{
	fields {
		field a {
			type: "dimension"
			label: "field a"
		}

		field b {
			type: "measure"
			label: "field b"
		}
	}

	options {
		option tooltip {
			type: 'toggle'
			label: 'Show tooltip'
			default_value: true
		}
		option bar_color {
			type: 'color-picker'
			label: 'Bar color'
			default_value: 'cyan'
		}
	}

	template: @vgl {
		"data": {
			"values": @{values}
		},
		"mark": {
			"type": "bar",
			"width": 20,
			"tooltip": @{options.tooltip.value},
			"color": @{options.bar_color.value}
		},
		"encoding": {
			"x": {
					"field": @{fields.a.name}, 
					"type": "temporal", 
					"axis": {
						"labelAngle": -45,
						"format": "%B of %Y"
						}
					},
			"y": {
				"field": @{fields.b.name}, 
				"type": "quantitative"
				}
			}
	};;
}
```
