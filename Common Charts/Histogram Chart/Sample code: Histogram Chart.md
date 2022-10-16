# Histogram Chart

### Goal

In this guide, we will create a Histogram Chart that subdivides a numerical range into bins, and counts the number of data points within each segment. The resulting bar chart provides a discrete estimate of the probability density function.

![](https://user-images.githubusercontent.com/27631976/196044744-b4df26a0-6fe9-4095-8b56-fbfd7e94b09d.png)

### Full code example

```jsx
CustomChart {
  fields {
    field data {
      type: 'dimension'
    }
  }
  template: @vgl
  {
    "data": {"values": @{values}},
    "transform": [
      {"bin": {"maxbins": 40}, "field": @{fields.data.name}, "as": "binned_price"}
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
