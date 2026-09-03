# Charts with Trendlines

Overlays a regression trend line (with a selectable functional form — linear, log, exp, pow, quad, poly) on top of a bar or scatter chart, with the R² value rendered as an on-chart text label. Two variants are included, differing only in the base mark.

## Variants

### Scatterplot with Trendline

![scatterplot-with-trendline](https://github.com/holistics/custom-chart-library/assets/106363759/72ac623a-da0d-42ed-920a-02d77014415e)

We use Vega-Lite's [layer](https://vega.github.io/vega-lite/docs/layer.html) property to display both the scatterplot and the trend line:

1. In the template's `layer` array, the first layer is for the scatterplot, and the second layer is for the trend line.
2. The scatterplot layer is a `point` mark encoding `x` and `y`.
3. The trend line layer uses `layer` again to combine the scatterplot with a `line` mark, calculating the trend line's coordinates via the [regression](https://vega.github.io/vega-lite/docs/regression.html) transform and encoding the line from the transformed data.

### Bar Chart with Trendline

![bar-with-trendline](https://github.com/holistics/custom-chart-library/assets/106363759/4ca65219-4ebe-46fe-8813-fea918f72631)

![bar-with-trendline final result](https://github.com/holistics/custom-chart-library/assets/106363759/2cad544d-034e-4c93-aca3-4fb07b516119)

## Regression methods

Both variants share the same `regression_method` option, which chooses the trend line's functional form:

- linear (`linear`): *y = a + b·x* (default)
- logarithmic (`log`): *y = a + b·log(x)*
- exponential (`exp`): *y = a·e^(b·x)*
- power (`pow`): *y = a·x^b*
- quadratic (`quad`): *y = a + b·x + c·x²*
- polynomial (`poly`): *y = a + b·x + … + k·x^order*

## Syntax reference

### As-code syntax

Not available.

### Legacy syntax

- [scatterplot_with_trendline.vgl.aml](legacy/scatterplot_with_trendline.vgl.aml)
- [bar_chart_with_trendline.vgl.aml](legacy/bar_chart_with_trendline.vgl.aml)

