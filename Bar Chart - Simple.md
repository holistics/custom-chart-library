# Simple Bar Chart

Final Result
![](https://i.imgur.com/JggMN3u.png)

```javascript
CustomChart {
	fields {
		field _dimension {
			type: "dimension"
			label: "Dimension"
		}

		field _measure {
			type: "measure"
			label: "Value"
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
			default_value: '#00FFFF'
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
					"field": @{fields._dimension.name}, 
					"type": "temporal", 
					"axis": {
						"labelAngle": -45,
						"format": "%B of %Y"
						}
					},
			"y": {
				"field": @{fields._measure.name}, 
				"type": "quantitative"
				}
			}
	};;
}
```
