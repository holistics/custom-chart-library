# Boxplot
A [box plot](https://vega.github.io/vega-lite/docs/boxplot.html#boxplot-types) summarizes a distribution of quantitative values using a set of summary statistics. The median tick in the box represents the median. The lower and upper parts of the box represent the first and third quartile respectively. Depending on the type of box plot, the ends of the whiskers can represent multiple things.

## Basic example

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190141668-afca099e-f1e2-4fcb-b451-3422f5b9b691.png">


## Basic example (exploration with tooltip; showing summaries)

<img width="761" alt="Screenshot 2022-09-14 113833" src="https://user-images.githubusercontent.com/27631976/190141067-ead4bef4-c404-4b8c-a9e5-c36d09696409.png">

## Box Plots with Options

Offering Box Plots that render the bars either vertically or horizonally. For each, the user may control:

* Outlier visibility
* Box width
* Box size
* Chart label font style
* Box color
* Median color
* Background color
* Extent (whiskers)

### Note on Extent and Box Plot Types

[Box Plot Types](https://vega.github.io/vega-lite/docs/boxplot.html#boxplot-types) supports either a integer or "min-max"

* **Integer**:  defines the **Tukey Box Plot**  the whisker spans from the smallest data to the largest data within the range *[Q1 - k * IQR, Q3 + k * IQR]* where Q1 and Q3 are the first and third quartiles while *IQR* is the interquartile range (Q3-Q1). In this type of box plot, you can specify the constant *k* by setting the extent. If there are outlier points beyond the whisker, they will be displayed using point marks.
 The default is 1.5
* **"min-max"**: The lower and upper whiskers are defined as the min and max respectively. No points will be considered as outliers for this type of box plots.

## Box Plot Horizontal:

![boxplot-horizontal](https://github.com/stonematt/i/assets/2821486/ef39e00a-0c2a-42af-a704-1dbe26edc6eb)

## Box Plot Vertical

![boxplot-vertical](https://github.com/stonematt/i/assets/2821486/c594ff96-dff4-45f3-a429-858020c11e04)
