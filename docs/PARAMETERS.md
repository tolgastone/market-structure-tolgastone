# Parameter reference

These names and defaults match the current [`ms_tolgastone.pine`](../ms_tolgastone.pine). The indicator exposes **33 inputs in five groups**. The shipped view emphasizes structure: Cement markers and FVG fragments are hidden; only optional Fibonacci ratios 0.5, 0.618 and 0.786 are enabled.

## Appearance — 3 inputs

| Setting | Default | Effect |
| --- | --- | --- |
| Color mode | Auto | Auto adapts the palette to chart-background brightness. Custom uses the color controls in all groups. Visibility settings work in either mode. |
| Bullish color | Teal `#087F7A` | In Custom mode, colors upward break lines/text, high circles and bullish Cement markers. |
| Bearish color | Coral `#BA4C52` | In Custom mode, colors downward break lines/text, low circles and bearish Cement markers. |

Auto uses darker tones on light backgrounds and lighter tones on dark backgrounds. Structure is blue; Fibonacci is muted slate; boundary arrows are faded. Automatic FVG fills have transparency 90 on light backgrounds and 87 on dark backgrounds. Color controls are retained but do not override the palette until Color mode is Custom.

## Structure — 2 inputs

| Setting | Default | Accepted values | Effect |
| --- | --- | --- | --- |
| Break source | Close | Close, High/Low | Uses current close or candle extremes for strict reference-boundary breaks. Equal touches do not break. Close does not imply evaluation only at bar close. |
| Cement lookback bars | 1 | Integer 1–10; step 1 | Bullish close must exceed every high of the preceding N bars; bearish close must fall below every low. Not a consecutive-Cement count. |

These two controls affect structural calculations. Changing them can change swing anchors, event classification and event timing.

## Structure display — 7 inputs

| Setting | Default | Effect |
| --- | --- | --- |
| Cement markers | Off | Shows the Cement markers emitted by the structural engine. Does not mark every qualifying Cement candle or disable calculations when off. |
| BOS / CHoCH labels | On | Shows both dashed break lines and their labels. Does not disable structural break processing. |
| Swing extreme markers | On | Shows high and low circles for drawn swing endpoints, using the bullish and bearish palette colors. |
| Working swing boundaries | On | Shows working high/low arrows. Break processing uses the reference swing, which can differ. |
| Structure line color | Blue `#4773A8` | In Custom mode, colors swing lines. Break lines and text use the directional colors. |
| Boundary color | Blue `#4773A8`; transparency 40 | In Custom mode, independently colors working swing boundary arrows. |
| Structure line width | 1 | Integer 1–5; step 1. Changes swing-line width. Dashed break lines and boundary arrows retain their fixed width. |

There is no master input for hiding historical swing lines. Boundary color is independent of Fibonacci color. Swing width defaults to 1; break lines and boundary arrows also remain thin.

## Fibonacci — 18 inputs

Each row below contains **two inputs**: an Enable switch and an editable Ratio. Only Enable 4, Enable 5 and Enable 7 default to On. Ratios have step 0.001 and no explicit minimum or maximum.

| Enable control | Ratio control | Default ratio | Enabled by default |
| --- | --- | --- | --- |
| Enable 1 | Ratio 1 | -0.272 | Off |
| Enable 2 | Ratio 2 | 0.236 | Off |
| Enable 3 | Ratio 3 | 0.382 | Off |
| Enable 4 | Ratio 4 | 0.5 | On |
| Enable 5 | Ratio 5 | 0.618 | On |
| Enable 6 | Ratio 6 | 0.700 | Off |
| Enable 7 | Ratio 7 | 0.786 | On |
| Enable 8 | Ratio 8 | 1.272 | Off |

Two further controls apply to the layer:

| Setting | Default | Effect |
| --- | --- | --- |
| Show Fibonacci levels | On | Shows logarithmic levels on the working swing. Hiding the layer does not change structure processing. |
| Fibonacci color | Slate `#64748B`; transparency 20 | In Custom mode, colors Fibonacci lines and text independently of boundary arrows. |

Levels 0 and 1 are always added when the Fibonacci layer is shown; they have no individual Enable controls. The editable levels are used in their configured order, without sorting or deduplication. Duplicate ratios can create overlapping drawings.

Prices are interpolated logarithmically. Ratio 0.5 is the geometric midpoint. Displayed 0.618 and 0.786 levels do not constitute a dedicated OTE zone. Very large ratio values can produce extreme coordinates; no additional ratio validation is implemented.

## Fair value gaps — 3 inputs

| Setting | Default | Effect |
| --- | --- | --- |
| Show FVG fragments | Off | Shows surviving fragments inside the reference swing's log-expanded display range. Tracking continues when hidden. |
| Bearish fragment color | Coral `#BA4C52`; transparency 90 | Appearance of bearish fragments in Custom mode. |
| Bullish fragment color | Teal `#087F7A`; transparency 90 | Appearance of bullish fragments in Custom mode. |

Color transparency uses the Pine scale: 0 is opaque and 100 is invisible. No gap-age, minimum-size, mitigation-mode or storage-limit inputs are provided.

## What the settings do not change

The current script has no timeframe selector, close-confirmation toggle, configurable structural alert conditions, EMA/SuperTrend settings, or previous-period level settings. Historical drawing limits and the FVG fragment lifecycle are not configurable through these inputs.

See the [usage guide](USAGE.md) for chart interpretation and [The MST approach](ALGORITHM.md) for the exact structural and FVG behavior.

## Chart title and Fibonacci contrast

Input values are hidden from the chart status line so the short title remains MST; all controls remain available in settings. Numeric/string inputs remain visible in the Data Window. In Auto mode on a dark background, Fibonacci text uses a brighter slate tone while the lines retain their subdued color. Light-background and Custom-mode Fibonacci colors are unchanged.
