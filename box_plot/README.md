# Box Plot

A [box plot](https://vega.github.io/vega-lite/docs/boxplot.html#boxplot-types) summarizes a distribution of quantitative values using a set of summary statistics. The median tick in the box represents the median. The lower and upper parts of the box represent the first and third quartile respectively. Depending on the type of box plot, the ends of the whiskers can represent multiple things.

- **Good for:** comparing distributions across categories, spotting spread and skew, surfacing outliers in a metric.
- **Not great for:** showing a single total or count per category (use a bar chart), trends over time (use a line chart), or part-to-whole composition (use a pie or treemap chart).

![](https://media.holistics.io/3331ae2e-box-plot.png)

## Required fields

| Field       | Label     | Type        | Role |
|-------------|-----------|-------------|------|
| `dimension` | Dimension | `dimension` | Category that splits the data into one box per group. |
| `value`     | Value     | `dimension` | Numeric observations the box plot summarizes into median, quartiles, and whiskers. |

## Note on Extent and Box Plot Types

[Box Plot Types](https://vega.github.io/vega-lite/docs/boxplot.html#boxplot-types) supports either a integer or "min-max"

- **Integer**:  defines the **Tukey Box Plot**  the whisker spans from the smallest data to the largest data within the range *[Q1 - k * IQR, Q3 + k * IQR]* where Q1 and Q3 are the first and third quartiles while *IQR* is the interquartile range (Q3-Q1). In this type of box plot, you can specify the constant *k* by setting the extent. If there are outlier points beyond the whisker, they will be displayed using point marks.
 The default is 1.5
- **"min-max"**: The lower and upper whiskers are defined as the min and max respectively. No points will be considered as outliers for this type of box plots.

## Known limitations

- **Needs raw rows, not aggregates.** The mark derives quartiles from the underlying values, so pre-aggregated data (one value per category) leaves nothing to summarize.
- **Each category needs enough observations.** Boxes built from very few rows give misleading quartiles and whiskers. Make sure each group has a meaningful number of points.
- **Whisker extent changes which points count as outliers.** A higher extent (or `min-max`) pulls the whiskers out and reclassifies points, so the same data can look outlier-free or outlier-heavy depending on the setting.

## Legacy chart

> [!NOTE]
>
> Legacy charts are still usable but no longer maintained. We suggest switching to the as-code charts instead.

The legacy chart provides three variants: a simple vertical box plot, plus richer vertical and horizontal variants with styling options.

### Variants

**Vertical (simple)** — fields `a` (category), `b` (value); a single `tooltip` option.

<img width="925" alt="image" src="https://user-images.githubusercontent.com/27631976/190141668-afca099e-f1e2-4fcb-b451-3422f5b9b691.png">

<img width="761" alt="Screenshot 2022-09-14 113833" src="https://user-images.githubusercontent.com/27631976/190141067-ead4bef4-c404-4b8c-a9e5-c36d09696409.png">

**Horizontal (rich)** — fields `group` (category), `measure` (value); full styling options (see below).

![boxplot-horizontal](https://github.com/stonematt/i/assets/2821486/ef39e00a-0c2a-42af-a704-1dbe26edc6eb)

**Vertical (rich)** — fields `group` (category), `measure` (value); full styling options (see below).

![boxplot-vertical](https://github.com/stonematt/i/assets/2821486/c594ff96-dff4-45f3-a429-858020c11e04)

### Options

Offering Box Plots that render the bars either vertically or horizonally. For each, the user may control:

- Outlier visibility
- Box width
- Box size
- Chart label font style
- Box color
- Median color
- Background color
- Extent (whiskers)

> Inspired by [Video Tutorial](https://docs.holistics.io/docs/charts/custom-charts#video-tutorial)

**Legacy code:**

- [boxplot.vgl.aml](legacy/boxplot.vgl.aml) (vertical, simple)
- [boxplot_horizontal.vgl.aml](legacy/boxplot_horizontal.vgl.aml) (horizontal, with styling options)
- [boxplot_vertical.vgl.aml](legacy/boxplot_vertical.vgl.aml) (vertical, with styling options)

## As-code chart

Two as-code variants are available, both taking the same two fields and the same `show_outliers` option:

- **Vertical Box Plot** (`box_plot`): boxes laid out top to bottom (the default orientation).
- **Horizontal Box Plot** (`box_plot_horizontal`): boxes laid out left to right.

### Options

| Option          | Default | Effect                                                       |
| --------------- | ------- | ------------------------------------------------------------ |
| `show_outliers` | `false` | Plots individual outlier points beyond the whiskers when on. |

*Need finer control? Edit the `.chart.aml` file directly to add more options.*

**As-code:**

- [box_plot.chart.aml](as-code/box_plot.chart.aml) (vertical)
- [box_plot_horizontal.chart.aml](as-code/box_plot_horizontal.chart.aml) (horizontal)
