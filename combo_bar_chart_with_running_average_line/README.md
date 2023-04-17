# Combo Chart: Bar Chart with Running Average Line

*Author: [**@ianbmc**](https://github.com/ianbmc)*

## Introduction

This is a template code for Bar Chart with a Line of Running Average.

![demo-bar-moving-avg](https://user-images.githubusercontent.com/106363759/232543243-11936d05-df3d-41df-963a-c924e4fcb1ab.png)

Similar to [Combo Chart: Bar Chart with Average Line](https://github.com/holistics/custom-chart-library/tree/main/combo_bar_chart_with_average_line), in this code we will create a combo that consists of:
- a Bar chart
- and an average line showing the moving (or running) mean of y-axis value

## Full Code Example

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
		option line_color {
			type: 'color-picker'
			label: 'Line color'
			default_value: '#00FFFF'
		}
    option points_before {
      type: 'number-input'
      label: 'Points before'
      default_value: -3
    }
    option points_after {
      type: 'number-input'
      label: 'Points after'
      default_value: 0
    }    
	}

	template: @vgl {
		"data": {
			"values": @{values}
		},
    "transform": [
    {
      "sort": [{"field": @{fields.dimension.name}}],
      "window": [{"op": "average", "field": @{fields.measure.name}, "as": "avg"}],
      "frame": [@{options.points_before.value}, @{options.points_after.value}]
    }
    ],
    "layer": [
		{"mark": {
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
  },
    {"mark": {
			"type": "line",
			"tooltip": @{options.tooltip.value},
			"color": @{options.line_color.value}
		},
  "encoding": {
    "x": {"field": @{fields.dimension.name}, "type": "nominal"},
    "y": {"field": "avg", "type": "quantitative"}
  }
  
  }
  ]};;
}

```
