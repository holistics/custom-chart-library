# Sankey Chart

## Goal
In this guide, we will create a **Sankey Chart** to depict how energy is converted before being consumed or lost, by visualizing the energy sources & their links' weight.

![Screen Shot 2023-03-16 at 12 16 23 PM](https://user-images.githubusercontent.com/124118181/225521638-9898c83d-b45e-4ca8-ba80-9a80d1f60d5b.png)

## Notes
Holistics’s Sankey diagram only supports acyclic link. Loop link is unavailable at the moment.

Meaning you cannot render Sankey chart where a node in a latter stage link back to another node in a previous stage. _e.g. node `A` link to node `B`, and node `B` link back to node `A`._

## Sample Data

💡 Import this data into Holistics to use: [sankey-sample-data.csv](https://github.com/holistics/custom-chart-library/files/11058203/sankey-sample-data.csv)

## Full Code Example

```javascript

CustomChart {
  fields {
    field source {
      type: "dimension"
      label: "Source Node"
      data_type: "string"
    }
    field target {
      type: "dimension"
      label: "Target Node"
      data_type: "string"
    }
    field volume {
      type: "dimension"
      label: "Volume for link from Source-Target"
      data_type: "number"
    }
  }
  options {
    option node_align {
      label: "Node Align"
      type: "select"
      options: ["justify", "left", "right", "center"]
      default_value: "justify"
    }
    option node_width {
      label: "Node Width"
      type: "number-input"
      default_value: 15
    }
    option node_padding {
      label: "Node Padding"
      type: "number-input"
      default_value: 10
    }
    option margin_top {
      label: "Margin Top"
      type: "number-input"
      default_value: 10
    }
    option margin_left {
      label: "Margin Left"
      type: "number-input"
      default_value: 10
    }
    option margin_right {
      label: "Margin right"
      type: "number-input"
      default_value: 10
    }
    option margin_bottom {
      label: "Margin bottom"
      type: "number-input"
      default_value: 10
    }
  }
  template: @vgl
  {
    "$schema": "https://vega.github.io/schema/vega/v5.json",
    "width": 1000,
    "height": 600,
    "autosize": "none",
    "data": [
      {
        "name": "table",
        "values": @{values}
      },
      {
        "name": "nodesAndLinks",
        "source": "table",
        "transform": [
          {
            "type": "formula",
            "expr": "width",
            "as": "containerWidth"
          },
          {
            "type": "formula",
            "expr": "height",
            "as": "containerHeight"
          },
          {
            "type": "sankey",
            "source": "datum['@{fields.source.name}']",
            "target": "datum['@{fields.target.name}']",
            "volume": "datum['@{fields.volume.name}']",
            "nodeAlign": @{options.node_align.value},
            "nodeWidth": @{options.node_width.value},
            "nodePadding": @{options.node_padding.value},
            "marginTop": @{options.margin_top.value},
            "marginRight": @{options.margin_right.value},
            "marginBottom": @{options.margin_bottom.value},
            "marginLeft": @{options.margin_left.value}
          },
          {
            "type": "formula",
            "expr": "datum.source.id",
            "as": "sourceId"
          },
          {
            "type": "formula",
            "expr": "datum.target.id",
            "as": "targetId"
          }
        ]
      },
      {
        "name": "links",
        "source": "nodesAndLinks",
        "transform": [
          {
            "type": "linkpath",
            "orient": "horizontal",
            "shape": "diagonal",
            "sourceY": {
              "expr": "datum.y0"
            },
            "sourceX": {
              "expr": "datum.source.x1"
            },
            "targetY": {
              "expr": "datum.y1"
            },
            "targetX": {
              "expr": "datum.target.x0"
            },
            "as": "path"
          },
          {
            "type": "formula",
            "expr": "datum.width",
            "as": "linkWidth"
          }
        ]
      },
      {
        "name": "extractedNode",
        "source": "nodesAndLinks",
        "transform": [
          {
            "type": "formula",
            "expr": "datum.source.id",
            "as": "sourceId"
          },
          {
            "type": "formula",
            "expr": "datum.source.x0",
            "as": "sourceX0"
          },
          {
            "type": "formula",
            "expr": "datum.source.x1",
            "as": "sourceX1"
          },
          {
            "type": "formula",
            "expr": "datum.source.y0",
            "as": "sourceY0"
          },
          {
            "type": "formula",
            "expr": "datum.source.y1",
            "as": "sourceY1"
          },
          {
            "type": "formula",
            "expr": "(datum.source.y0 + datum.source.y1)/2",
            "as": "sourceYc"
          },
          {
            "type": "formula",
            "expr": "datum.target.id",
            "as": "targetId"
          },
          {
            "type": "formula",
            "expr": "datum.target.x0",
            "as": "targetX0"
          },
          {
            "type": "formula",
            "expr": "datum.target.x1",
            "as": "targetX1"
          },
          {
            "type": "formula",
            "expr": "datum.target.y0",
            "as": "targetY0"
          },
          {
            "type": "formula",
            "expr": "datum.target.y1",
            "as": "targetY1"
          },
          {
            "type": "formula",
            "expr": "(datum.target.y0 + datum.target.y1)/2",
            "as": "targetYc"
          }
        ]
      },
      {
        "name": "uniqueSource",
        "source": "extractedNode",
        "transform": [
          {
            "type": "aggregate",
            "groupby": [
              "sourceId",
              "sourceX0",
              "sourceX1",
              "sourceY0",
              "sourceY1",
              "sourceYc"
            ]
          }
        ]
      },
      {
        "name": "uniqueTarget",
        "source": "extractedNode",
        "transform": [
          {
            "type": "aggregate",
            "groupby": [
              "targetId",
              "targetX0",
              "targetX1",
              "targetY0",
              "targetY1",
              "targetYc"
            ]
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
      }
    ],
    "scales": [
      {
        "name": "sourceColor",
        "type": "ordinal",
        "range": "category",
        "domain": {
          "data": "nodesAndLinks",
          "field": "sourceId"
        }
      },
      {
        "name": "targetColor",
        "type": "ordinal",
        "range": "category",
        "domain": {
          "data": "nodesAndLinks",
          "field": "targetId"
        }
      }
    ],
    "marks": [
      {
        "type": "path",
        "name": "edgeMark",
        "from": {
          "data": "links"
        },
        "clip": true,
        "encode": {
          "update": {
            "path": {
              "field": "path"
            },
            "strokeWidth": {
              "field": "linkWidth"
            },
            "stroke": [
              {
                "scale": "sourceColor",
                "field": "sourceId"
              }
            ],
            "strokeOpacity": {
              "value": 0.6
            },
            "tooltip": {
              "signal": "datum.sourceId + ' → ' + datum.targetId + ': ' + datum.value"
            }
          },
          "hover": {
            "strokeOpacity": {
              "value": 1
            }
          }
        }
      },
      {
        "type": "rect",
        "name": "targetMark",
        "from": {
          "data": "uniqueTarget"
        },
        "encode": {
          "update": {
            "x": {
              "field": "targetX0"
            },
            "x2": {
              "field": "targetX1"
            },
            "y": {
              "field": "targetY0"
            },
            "y2": {
              "field": "targetY1"
            },
            "fill": [
              {
                "scale": "targetColor",
                "field": "targetId"
              }
            ],
            "stroke": {
              "value": "#000"
            },
            "strokeWidth": {
              "value": 0.5
            }
          },
          "hover": {
            "strokeWidth": {
              "value": 3
            }
          }
        }
      },
      {
        "type": "rect",
        "name": "sourceMark",
        "from": {
          "data": "uniqueSource"
        },
        "encode": {
          "update": {
            "x": {
              "field": "sourceX0"
            },
            "x2": {
              "field": "sourceX1"
            },
            "y": {
              "field": "sourceY0"
            },
            "y2": {
              "field": "sourceY1"
            },
            "fill": [
              {
                "scale": "sourceColor",
                "field": "sourceId"
              }
            ],
            "stroke": {
              "value": "#000"
            },
            "strokeWidth": {
              "value": 0.5
            }
          },
          "hover": {
            "strokeWidth": {
              "value": 3
            }
          }
        }
      },
      {
        "type": "text",
        "name": "sourceTextMark",
        "from": {
          "data": "uniqueSource"
        },
        "interactive": false,
        "encode": {
          "update": {
            "yc": {
              "field": "sourceYc"
            },
            "x": {
              "signal": "datum.sourceX1 > width / 2 ? datum.sourceX0 - 5 : datum.sourceX1 + 5"
            },
            "align": {
              "signal": "datum.sourceX1 > width / 2 ? 'right' : 'left'"
            },
            "baseline": {
              "value": "middle"
            },
            "fontWeight": {
              "value": "normal"
            },
            "text": {
              "field": "sourceId"
            }
          }
        }
      },
      {
        "type": "text",
        "name": "targetTextMark",
        "from": {
          "data": "uniqueTarget"
        },
        "interactive": false,
        "encode": {
          "update": {
            "yc": {
              "field": "targetYc"
            },
            "x": {
              "signal": "datum.targetX1 > width / 2 ? datum.targetX0 - 5 : datum.targetX1 + 5"
            },
            "align": {
              "signal": "datum.targetX1 > width / 2 ? 'right' : 'left'"
            },
            "baseline": {
              "value": "middle"
            },
            "fontWeight": {
              "value": "normal"
            },
            "text": {
              "field": "targetId"
            }
          }
        }
      }
    ]
  };;
}

```
