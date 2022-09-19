# Create a combo chart

## Introduction

Combo chart (or combination chart, mixed type chart,...) are useful to display the differences between different sets of data. 

In this guide, we will demonstrate on how to create a combo chart with [Custom Chart](https://docs.holistics.io/docs/charts/custom-charts) by going through the process of creating a Bar Chart with an average line showing the mean of y-axis value.

## Mechanism

We will use [`layer`](https://vega.github.io/vega-lite/docs/layer.html) property in Vega-lite to display multiple charts into a unified view.

## Step-by-step instructions

1. In your **Template** section, add a **layer** field. Recall that this field takes in an array. The first layer  is for the bar chart, and the second layer is for the mean/average line. 

```json
template: @vgl {
    ...
    "layer": [{ // add your bar chart definition here },
                        { // add your line chart definition here }]
    ...	
}
```
    
2. Add the **Bar Chart** definition as the first element of the **layer** array.
    
```json
template: @vgl {
    ...
    "layer": [{
    "mark": {
        "type": "bar",
    },
    "encoding": {
        "x": {
        "field": @{fields.x.name},
        },
        "y": {
        "field": @{fields.y.name},
        "type": "quantitative",
        },
    }
    }, {
    // add your line chart here
    }],
};;
```
    
3. Add the **Line Chart** definition as the second element of the **layer** array.
    
```json
template: @vgl {
    "data": {"values": @{values}},
    "layer": [{
            // bar chart definition is omitted for brevity
        }, {
        "mark": "rule",
        "encoding": {
        "y": {
            "aggregate": "mean",
            "field": @{fields.y.name},
            "type": "quantitative"
            }
            }
        }],
    };;
```
