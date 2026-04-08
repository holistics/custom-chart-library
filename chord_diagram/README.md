# Chord Diagram

## Goal
In this guide, we will create a **Chord Diagram** to visualize the inter-relationships and flows between different entities, by showing weighted connections (chords) between nodes arranged in a circular layout.

<!-- TODO: Add screenshot -->

## Sample Data

💡 Sankey sample data can be reused: [sankey-sample-data.csv](https://github.com/holistics/custom-chart-library/files/11058203/sankey-sample-data.csv)

## Full Code Example

```javascript
CustomChart {
  fields {
    field source {
      type: "dimension"
      label: "Source"
      data_type: "string"
    }
    field target {
      type: "dimension"
      label: "Target"
      data_type: "string"
    }
    field value {
      type: "measure"
      label: "Flow value"
    }
  }
  options {
    option pad_angle {
      label: "Pad Angle"
      type: "number-input"
      default_value: 0.05
    }
    option inner_radius_ratio {
      label: "Inner Radius Ratio"
      type: "number-input"
      default_value: 0.9
    }
    option label_padding {
      label: "Label Padding"
      type: "number-input"
      default_value: 80
    }
  }
  template: @vgl
  {
    "$schema": "https://vega.github.io/schema/vega/v5.json",
    "width": 600,
    "height": 600,
    "autosize": "none",
    "data": [
      {
        "name": "table",
        "values": @{values}
      },
      {
        "name": "chordData",
        "source": "table",
        "transform": [
          {
            "type": "chord",
            "source": "datum['@{fields.source.name}']",
            "target": "datum['@{fields.target.name}']",
            "value": "datum['@{fields.value.name}']",
            "padAngle": @{options.pad_angle.value},
            "innerRadiusRatio": @{options.inner_radius_ratio.value},
            "labelPadding": @{options.label_padding.value}
          }
        ]
      },
      {
        "name": "uniqueGroups",
        "source": "chordData",
        "transform": [
          {
            "type": "formula",
            "expr": "datum.sourceGroup.id",
            "as": "groupId"
          },
          {
            "type": "formula",
            "expr": "datum.sourceGroup.startAngle",
            "as": "groupStartAngle"
          },
          {
            "type": "formula",
            "expr": "datum.sourceGroup.endAngle",
            "as": "groupEndAngle"
          },
          {
            "type": "formula",
            "expr": "(datum.sourceGroup.startAngle + datum.sourceGroup.endAngle) / 2",
            "as": "groupMidAngle"
          },
          {
            "type": "formula",
            "expr": "datum.sourceGroup.value",
            "as": "groupValue"
          },
          {
            "type": "aggregate",
            "groupby": ["groupId", "groupStartAngle", "groupEndAngle", "groupMidAngle", "groupValue"]
          }
        ]
      },
      {
        "name": "targetGroups",
        "source": "chordData",
        "transform": [
          {
            "type": "formula",
            "expr": "datum.targetGroup.id",
            "as": "groupId"
          },
          {
            "type": "formula",
            "expr": "datum.targetGroup.startAngle",
            "as": "groupStartAngle"
          },
          {
            "type": "formula",
            "expr": "datum.targetGroup.endAngle",
            "as": "groupEndAngle"
          },
          {
            "type": "formula",
            "expr": "(datum.targetGroup.startAngle + datum.targetGroup.endAngle) / 2",
            "as": "groupMidAngle"
          },
          {
            "type": "formula",
            "expr": "datum.targetGroup.value",
            "as": "groupValue"
          },
          {
            "type": "aggregate",
            "groupby": ["groupId", "groupStartAngle", "groupEndAngle", "groupMidAngle", "groupValue"]
          }
        ]
      },
      {
        "name": "allGroups",
        "source": ["uniqueGroups", "targetGroups"],
        "transform": [
          {
            "type": "aggregate",
            "groupby": ["groupId", "groupStartAngle", "groupEndAngle", "groupMidAngle", "groupValue"]
          }
        ]
      }
    ],
    "signals": [
      {
        "name": "width",
        "init": "(containerSize()[0])",
        "on": [
          {
            "update": "(containerSize()[0])",
            "events": "window:resize"
          }
        ]
      },
      {
        "name": "height",
        "init": "(containerSize()[1])",
        "on": [
          {
            "update": "(containerSize()[1])",
            "events": "window:resize"
          }
        ]
      },
      {
        "name": "labelPadding",
        "update": "@{options.label_padding.value}"
      },
      {
        "name": "outerRadius",
        "update": "max(min(width, height) / 2 - labelPadding, 10)"
      },
      {
        "name": "innerRadius",
        "update": "outerRadius * @{options.inner_radius_ratio.value}"
      }
    ],
    "scales": [
      {
        "name": "color",
        "type": "ordinal",
        "range": "category",
        "domain": {
          "data": "allGroups",
          "field": "groupId"
        }
      }
    ],
    "marks": [
      {
        "type": "group",
        "encode": {
          "update": {
            "x": {"signal": "width / 2"},
            "y": {"signal": "height / 2"}
          }
        },
        "marks": [
          {
            "type": "arc",
            "name": "groupArc",
            "from": {"data": "allGroups"},
            "encode": {
              "update": {
                "startAngle": {"field": "groupStartAngle"},
                "endAngle": {"field": "groupEndAngle"},
                "innerRadius": {"signal": "innerRadius"},
                "outerRadius": {"signal": "outerRadius"},
                "fill": {"scale": "color", "field": "groupId"},
                "stroke": {"value": "#fff"},
                "strokeWidth": {"value": 0.5},
                "tooltip": {"signal": "datum.groupId + ': ' + datum.groupValue"}
              },
              "hover": {
                "fillOpacity": {"value": 0.8}
              }
            }
          },
          {
            "type": "path",
            "name": "chordRibbon",
            "from": {"data": "chordData"},
            "encode": {
              "update": {
                "path": {"field": "ribbonPath"},
                "fill": {"scale": "color", "field": "sourceId"},
                "fillOpacity": {"value": 0.67},
                "stroke": {"value": "#fff"},
                "strokeWidth": {"value": 0.5},
                "tooltip": {"signal": "datum.sourceId + ' → ' + datum.targetId + ': ' + datum.chordValue"}
              },
              "hover": {
                "fillOpacity": {"value": 0.9}
              }
            }
          },
          {
            "type": "text",
            "name": "groupLabel",
            "from": {"data": "allGroups"},
            "encode": {
              "update": {
                "x": {"signal": "(outerRadius + 5) * cos((datum.groupMidAngle) - PI / 2)"},
                "y": {"signal": "(outerRadius + 5) * sin((datum.groupMidAngle) - PI / 2)"},
                "align": {"signal": "datum.groupMidAngle > PI ? 'right' : 'left'"},
                "baseline": {"value": "middle"},
                "fontWeight": {"value": "normal"},
                "fontSize": {"value": 11},
                "text": {"field": "groupId"},
                "angle": {"signal": "datum.groupMidAngle > PI ? (datum.groupMidAngle - PI / 2) * 180 / PI - 180 : (datum.groupMidAngle - PI / 2) * 180 / PI"},
                "limit": {"signal": "max(labelPadding - 10, 20)"},
                "ellipsis": {"value": "…"},
                "tooltip": {"field": "groupId"}
              }
            }
          }
        ]
      }
    ]
  };;
}
```
