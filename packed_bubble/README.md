# Packed bubble
A custom chart based on [Vega Packed Bubble Chart Example](https://vega.github.io/vega/examples/packed-bubble-chart/)

## Example


## Fields
* Category (dimension)
* Value (measure)

## Options
* Fill color (color-scale): The scale to use for coloring bubbles
  * category
  * ramp
  * heatmap
* Gravity X (select)
* Gravity Y (select)
* Size scale - Min (number-input): The lower bound of bubble sizes
* Size scale - Max (number-input): The upper bound of bubble sizes. Adjust this value based on the real values of your measure to get desired bubble sizes.
* Text length limit (number-input): The max length (px) of bubble label. Lable longer than this limit will be truncated

## Code
[packed_bubble.vl.aml](packed_bubble.vg.aml)
