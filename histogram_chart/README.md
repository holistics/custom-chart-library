# Histogram Chart

This is an updated and backwards-compatible single-series histogram chart for Holistics, built with Vega-Lite.

It bins a numeric field and counts a dimension per bin using `mark: bar`. Axis labels are dynamically generated based on the selected fields.

---

## ✅ Features

- 📊 Single-series histogram
- 🧮 Configurable number of bins
- 🏷️ Automatic axis labeling:
  - X-axis: uses the binned field name
  - Y-axis: "Count of {grain field}"
- 🖋 Optional chart title override

---

## 📥 Fields

| Field    | Type                 | Description                                 |
| -------- | -------------------- | ------------------------------------------- |
| `grain`  | Dimension            | The dimension to count (e.g. Examiner Name) |
| `metric` | Dimension or Measure | The numeric field to bin (e.g. hours/day)   |

---

## 🧰 Options

| Option        | Type         | Default       | Description             |
| ------------- | ------------ | ------------- | ----------------------- |
| `max_bins`    | number-input | `20`          | Maximum number of bins  |
| `chart_title` | input        | `"Histogram"` | Optional title override |

---

## 🔎 Example Output

A simple bar histogram with dynamic binning and axis labels:

- Y-axis: `"Count of Examiner Name"`
- X-axis: `"Median Examiner hr/d binned"`

---

## 📁 Files

- `README.md` – this documentation
- `vgl.aml` – the Holistics CustomChart definition

---

## 🧠 Notes

If you want to compare **multiple series** in a histogram (e.g. by time period or event type), check out [`histogram_multi_series`](../histogram_multi_series).
