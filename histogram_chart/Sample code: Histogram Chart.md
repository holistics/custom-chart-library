# Histogram Chart

### Goal

In this guide, we will create a Histogram Chart that subdivides a numerical range into bins, and counts the number of data points within each segment. The resulting bar chart provides a discrete estimate of the probability density function.

![histogram_chart](https://user-images.githubusercontent.com/27631976/198895261-367ac1e3-ceee-4f6b-8b19-f0f501034d4b.png)

### Full code example

```jsx
CustomChart {
  fields {
    field grain {
      type: 'dimension'
    }
    field metric {
      type: 'dimension' // this type can be dimenson or measure depends on the type of field that you want to create the histogram on
    }
  }
  template: @vgl
  {
    "data": {"values": @{values}},
    "transform": [
      {"bin": {"maxbins": 40}, "field": @{fields.metric.name}, "as": "binned_price"}
    ],
    "mark": {
      "type": "bar",
      "tooltip": true
    },
    "encoding": {
      "x": {"field": "binned_price", "type": "quantitative", "bin": {"binned": true, "step": 20}},
      "x2": {"field": "binned_price_end"},
      "y": {"aggregate": "count"}
    }
  }
  ;;
}
```

### Applying the chart

![applying_histogram_custom_chart](https://user-images.githubusercontent.com/27631976/198895642-e79649d7-b790-40c0-8965-d6eb018d8be5.png)

