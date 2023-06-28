# Scatterplot with Trendline Tutorial

## Introduction

In this guide, we will guide you on how to add a regression trendline over a scatterplot that models field `"y"` as a function of `"x"` 

Supported functions included:

- linear (`linear`): *y = a + b * x (default value)*
- logarithmic (`log`): *y = a + b * log(x)*
- exponential (`exp`): *y = a * e^(b * x)*
- power (`pow`): *y = a * x^b*
- quadratic (`quad`): *y = a + b * x + c * x^2*
- polynomial (`poly`): *y = a + b * x + … + k * x^(order)*

Here is the result of a linear regression trend line:

![linear regression trend line](https://user-images.githubusercontent.com/27631976/194109892-746c5fb4-74f2-4616-a714-8a3276149611.png)

## Mechanism

We will use [layer](https://vega.github.io/vega-lite/docs/layer.html) property in Vega-lite to display both the scatterplot and the trend line.

## Step-by-step instructions

1. In your **Template** section, add a **layer** field. Recall that this field takes in an array. The first layer is for the scatterplot, and the second layer is for the trend line.

```js
template: @vgl {
    ...
    "layer": [{ // add your scatterplot definition here },
             { // add your trendline definition here }]
    ...	
}
```

2. Add the **scatterplot** definition as the first element of the **layer** array.

```js
{
 "mark": {
    "type": "point",
    "filled": true,
    "tooltip": true
  },
  "encoding": {
    "x": {
      "field": @{fields.x.name},
      "type": "quantitative",
      "axis": {
        "format": @{fields.x.format},
        "formatType": "holisticsFormat"
      }
    },
    "y": {
      "field": @{fields.y.name},
      "type": "quantitative",
      "axis": {
        "format": @{fields.y.format},
        "formatType": "holisticsFormat"
      }
    }
  }
}
```

3. Add the **Trend Line** definition as the second element of the **layer** array.

Now we want to add a line element, thus, we use `layer` to include both the scatterplot and the line:

```json
"layer": [
  {
    "mark": {
      "type": "point",
      "filled": true,
      "tooltip": true
    },
    "encoding": {
      "x": {
        "field": @{fields.x.name},
        "type": "quantitative",
        "axis": {
          "format": @{fields.x.format},
          "formatType": "holisticsFormat"
        }
      },
      "y": {
        "field": @{fields.y.name},
        "type": "quantitative",
        "axis": {
          "format": @{fields.y.format},
          "formatType": "holisticsFormat"
        }
      }
    }
  },
  {
    "mark": {
      "type": "line",
      "color": "firebrick"
    }
	}
]
```

We also need to calculate the coordinates (using the [regression](https://vega.github.io/vega-lite/docs/regression.html) transform) and encode the line properties using the transformed data:

```js
{
    "mark": {
      "type": "line",
      "color": "firebrick"
    },
    "transform": [
      {
        "regression": @{fields.y.name},
        "on": @{fields.x.name},
        "method": @{options.regression_method.value} // the regression method
      }
    ],
    "encoding": {
      "x": {
        "field": @{fields.x.name},
        "type": "quantitative"
      },
      "y": {
        "field": @{fields.y.name},
        "type": "quantitative"
      }
    }
 }
```


## Chose the regression method

You could change the functional form of the regression model by setting it in the `STYLES` tab.
![style](https://user-images.githubusercontent.com/27631976/194110238-038a8c8d-7475-441c-bfad-2528f519143e.jpg)


## Full code example

See [scatterplot_with_trendline.vgl.aml](./scatterplot_with_trendline.vgl.aml) for a full code example.
