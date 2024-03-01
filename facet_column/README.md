# Facet Column Chart

A Trellis plot (or small multiple) is a series of similar plots that displays different subsets of the same data, facilitating comparison across subsets.

The facet operator is one of Vega-Lite’s view composition operators. This is the most flexible way to create faceted plots and allows composition with other operators.

https://vega.github.io/vega-lite/docs/facet.html

## Custom Facet Column Chart in Holistics

**Generate an array of individual column charts.**

## Example

![facet-column](https://github.com/stonematt/i/assets/2821486/5c9fa343-e3a3-4686-9367-0ec7c3e18f44)

### Fields

* Facets to group into rows
* X-axis (categorical dimension)
    > works with time dimensions, but formating isn't there yet
* Metric to measure

### Options

Vega-lite sizing and scaling with facet charts has limited support for autosizing based on the interplay between unknown facet (row) count and thes size of the dashboard container.

Until that improves, this custom chart defintion includes support manually scaling the size and spacing of the component parts.

* Toggle Y-axis display
* Row sort prefernce
* Height of Row (in pixels)
* Width of the chart (in pixels)
* Space between rows (in pixels)
* Fill Color

