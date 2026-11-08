# Chart examples

**BINANCE:BTCUSDT · 1W · standard candles · UTC**. Break source: **Close**. Cement lookback: **1**. Color mode: **Auto**.

The following charts show the default layout and optional display layers on light and dark backgrounds.

## Default view

Swings, extreme circles, BOS/CHoCH, working boundaries and Fibonacci are shown. Optional ratios 0.5, 0.618 and 0.786 are enabled, in addition to fixed 0 and 1. Cement and FVG visibility are off.

![MST default view on a light background](default-light.png)

![MST default view on a dark background](default-dark.png)

## Optional Cement and FVG layers

The same structural settings with Cement markers and FVG fragments enabled. The chart framing differs from the default examples. FVG boxes extend from the current bar; they do not mark historical gap-origin positions.

![MST with Cement and FVG enabled on a light background](cement-fvg-light.png)

![MST with Cement and FVG enabled on a dark background](cement-fvg-dark.png)

The last weekly candle was open when these charts were captured. Its price and provisional drawings can change before close.

## Swing completion and break candles

The [illustrated concepts guide](../docs/CONCEPTS.md) walks through upward and downward impulse completion using the following chart excerpts. Yellow arrows identify the relevant Cement or break candles. The excerpts are cropped and have no full time axis; use them to read the structural sequence, not to infer exact event dates or intrabar availability.

- [UP impulse before Cement](swing-up-before-cement.png) and [after bearish Cement](swing-up-after-cement.png).
- [DOWN impulse before Cement](swing-down-before-cement.png) and [after bullish Cement](swing-down-after-cement.png).
- [Actual BOS/CHoCH break candles](break-candles.png).

## Swing Zones — v1.1.0

These examples show the new working-swing display layer with FVG also enabled. They are separate from the v1.0.0 default examples above; they are visual illustrations, not trading instructions.

- [NYSE:XYZ · 1W · UP / light](zones-up-light.png): high 99.26, low 38.85, logarithmic EQ approximately 62.10.
- [NASDAQ:FLYW · 1W · DOWN / light](zones-down-light.png) and [dark](zones-down-dark.png): high 35.80, low 8.20, EQ approximately 17.13 and 0.700 approximately 23.01.
- [Nasdaq-100 · 1W · DOWN / dark](zones-index-dark.png): OTE in Premium, with FVG fragments displayed separately.

See the [Swing Zones guide](../docs/SWING_ZONES.md) for anchors, visibility and timing.
