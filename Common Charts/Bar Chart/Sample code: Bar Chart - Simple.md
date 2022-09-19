# Simple Bar Chart

Final Result
![](https://i.imgur.com/JggMN3u.png)

```javascript
CustomChart {
	fields {
		field dimension {
			type: "dimension"
			label: "Dimension"
		}

		field measure {
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
			"tooltip": @{options.tooltip.value},
			"color": @{options.bar_color.value}
		},
		"encoding": {
			"x": {
				"field": @{fields.dimension.name}, 
				"type": "temporal", 
				"axis": {
					"labelAngle": -45,
					"format": @{fields.dimension.format},
					"formatType": "holisticsFormat"
				}
			},
			"y": {
				"field": @{fields.measure.name}, 
				"type": "quantitative",
				"axis": {
					"format": @{fields.measure.format},
					"formatType": "holisticsFormat"
				}
			}
		}
	};;
}
```
