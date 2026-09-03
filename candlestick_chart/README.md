# Candlestick Chart

A candlestick chart is essentially a sequence of box-plot-like bars placed side by side. It is commonly used to visualize the price movement of financial instruments such as forex, stocks, and bonds.

- **Good for:** open-high-low-close price movement over time, stock and forex trading sessions, daily or weekly trading ranges.
- **Not great for:** a single value per period (use a line or bar chart), non-time-series data, or categories without high and low bounds.

<img alt="reporting-custom-chart/candlestick" title="reporting-custom-chart/candlestick" src="https://media.holistics.io/e39cf75c-candlestick-chart.png" width="900" height="600" />

## Required fields

A Candlestick Chart expects exactly five fields. Each row of input is one period (one candle) with its open, high, low, and close values.

| Field   | Label | Type        | Role |
|---------|-------|-------------|------|
| `date`  | Date  | `dimension` | Time period for each candle; sets the x position. Sorted ascending (`apply_order: 1`). |
| `low`   | Low   | `measure`   | Lowest price; bottom of the wick. Sorted ascending (`apply_order: 2`). |
| `high`  | High  | `measure`   | Highest price; top of the wick. Sorted ascending (`apply_order: 3`). |
| `open`  | Open  | `measure`   | Opening price; one end of the candle body. Sorted ascending (`apply_order: 4`). |
| `close` | Close | `measure`   | Closing price; the other end of the candle body. Sorted ascending (`apply_order: 5`). |

**Data requirements:** Pre-aggregate to one row per date; the template does not combine duplicate periods. Each row needs all four price values, and the template colors a candle with the green option when `open` is less than `close` (otherwise the red option).

## Options

| Option         | Default | Effect |
|----------------|---------|--------|
| `tooltip`      | `true`  | Shows a tooltip on hover over each candle body. |
| `green_candle` | `green` | Fill color for up periods, where `close` is higher than `open`. |
| `red_candle`   | `red`   | Fill color for down periods, where `close` is at or below `open`. |

## Known limitations

- **Every row needs all four price values.** Each candle reads `open`, `high`, `low`, and `close`. Rows missing any of these render incompletely.
- **One row per period.** The template does not aggregate, so duplicate dates draw overlapping candles. Pre-aggregate to a single open-high-low-close row per period.
- **The y-axis does not start at zero.** The scale is set to fit the price range (`zero: false`), which is right for price data but means bar lengths are not proportional to absolute value.

## Syntax reference

### As-code syntax
- [candlestick_chart.chart.aml](as-code/candlestick_chart.chart.aml)

### Legacy syntax
- [candlestick_chart.vgl.aml](legacy/candlestick_chart.vgl.aml)
