# Custom Gauge Chart
---
![Custom Gauge Chart](https://github.com/truongthinhnguyen03/custom-chart-library/assets/121245100/428152b8-732a-4bca-9df7-236bb319cdf4)


# Code
---
```
CustomChart {
  fields {
    field current {
      type: "measure"
      label: "Current Value"
    }
    field max {
      type: "measure"
      label: "Max Value"
    }
  }

  options {
    option ring_max {
      type: "number-input"
      label: "Ring Max"
      default_value: 120
    }
    option ring_width {
      type: "number-input"
      label: "Ring Width"
      default_value: 25
    }
    option ring_gap {
      type: "number-input"
      label: "Ring Gap"
      default_value: 5
    }
    option ring_corner {
      type: "number-input"
      label: "Ring Corner"
      default_value: 10
    }
    option label_size {
      type: "number-input"
      label: "Label Size"
      default_value: 48
    }
    option label_color {
      type: "color-picker"
      label: "Label Color"
      default_value: "#53586A"
    }
    option ring1_color {
      type: "color-picker"
      label: "Ring Color"
      default_value: "#255DD4"
    }
  }
  
  template: @vg {
    "data": {
      "name": "dataset",
      "values": @{values}
    },
    "transform": [
      {"calculate": "datum['@{fields.current.name}']", "as": "current" },
      {"calculate": "datum['@{fields.max.name}']", "as": "max" },
      {"calculate": "datum.current / datum.max", "as": "_percentage"},
      {"calculate": "360 - ( 300 / 2 )", "as": "_arc_start_degrees"},
      {"calculate": "0 + ( 300 / 2 )","as": "_arc_end_degrees"},
      {"calculate": "2 * PI * ( datum['_arc_start_degrees'] - 360 ) / 360","as": "_arc_start_radians"},
      {"calculate": "2 * PI * datum['_arc_end_degrees'] / 360","as": "_arc_end_radians"},
      {"calculate": "datum['_arc_end_radians'] - datum['_arc_start_radians']","as": "_arc_total_radians"},
      {"calculate": "datum['_arc_start_radians']","as": "_ring_start_radians"},
      {"calculate": "datum['_arc_start_radians'] + ( datum['_arc_total_radians'] * datum['_percentage'] )","as": "_ring_end_radians"}
    ],
    "params": [
      {"name": "ring_max", "value": @{options.ring_max.value}},
      {"name": "ring_width", "value": @{options.ring_width.value}},
      {"name": "ring_gap", "value": @{options.ring_gap.value}},
      {"name": "ring_corner", "value": @{options.ring_corner.value}},
      {"name": "ring0_color", "value": "#FFFFFF"},
      {"name": "ring1_color", "value": @{options.ring1_color.value}},
      {"name": "label_color", "value": @{options.label_color.value}},
      {"name": "ring_background_opacity", "value": 0.3},
      {"name": "ring0_percent", "value": 100},
      {"name": "ring0_outer", "expr": "ring_max+2"},
      {"name": "ring0_inner", "expr": "ring_max+1"},
      {"name": "ring1_outer", "expr": "ring0_inner-ring_gap"},
      {"name": "ring1_inner", "expr": "ring1_outer-ring_width"},
      {"name": "ring1_middle", "expr": "(ring1_outer+ring1_inner)/2"}
    ],
    "layer": [
      {
        "name": "FULL RING",
        "mark": {
          "type": "arc",
          "radius": {
            "expr": "ring1_outer"
          },
          "radius2": {
            "expr": "ring1_inner"
          },
          "theta": {
            "expr": "datum['_arc_start_radians']"
          },
          "theta2": {
            "expr": "datum['_arc_end_radians']"
          },
          "cornerRadius": {
            "expr": "ring_corner"
          }
        },
        "encoding": {
          "color": {
            "value": {
              "expr": "ring1_color"
            }
          },
          "opacity": {
            "value": {
              "expr": "ring_background_opacity"
            }
          }
        }
      },
      {
        "name": "RING",
        "mark": {
          "type": "arc",
          "radius": {
            "expr": "ring1_outer"
          },
          "radius2": {
            "expr": "ring1_inner"
          },
          "theta": {
            "expr": "datum['_ring_start_radians']"
          },
          "theta2": {
            "expr": "datum['_ring_end_radians']"
          },
          "cornerRadius": {
            "expr": "ring_corner"
          }
        },
        "encoding": {
          "color": {
            "value": {
              "expr": "ring1_color"
            }
          }
        }
      },
      {
        "description": "VALUE",
        "mark": {
          "type": "text",
          "fontSize": @{options.label_size.value},
          "font": "Inter",
          "fontWeight": 500          
        },
        "encoding": {
          "text": {
            "field": "_percentage",
            "type": "quantitative",
            "format": ".02%"
          },
          "color": {
            "value": {
              "expr": "label_color"
            }
          }
        }
      }
    ],
    "background": "transparent",
  };;
}
```
