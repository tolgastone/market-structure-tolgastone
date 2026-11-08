# Market Structure by tolgastone

I developed MST for my own chart analysis and am sharing it as an open-source tool. I built the approach around Cement: a simple close-based rule for identifying when a directional impulse has completed. The aim is to make the structural sequence visible: a boundary breaks, price extends its extreme, and opposite Cement confirms completion.

## What is Cement?

With the default lookback of 1, bullish Cement closes above the previous candle's high; bearish Cement closes below its low. Body color does not define Cement. A close that only touches the boundary does not qualify.

A larger lookback compares the close with every high or low in that preceding window. It does not require consecutive Cement candles.

## Reading the structure

MST follows stored swing boundaries and labels normal structural breaks as:

- BOS — Break of Structure: a break in the reference swing's direction.
- CHoCH — Change of Character: a break against the reference swing's direction.

An UP impulse normally completes on bearish Cement; a DOWN impulse normally completes on bullish Cement. The first Cement establishes the initial direction. An opposite-boundary break can interrupt an active impulse before Cement completion.

## Extreme bar versus completion bar

An upward CHoCH can be followed by a rising impulse whose high remains provisional. When opposite bearish Cement completes the impulse, MST draws the swing to the earlier high bar. The mirrored sequence applies to a downward BOS impulse completed by bullish Cement. A completion candle can therefore appear after the actual price extreme.

The correction following completion is distinct from a new structural reversal: a new CHoCH still needs the appropriate boundary break. On each dashed BOS/CHoCH line, the right endpoint identifies the break candle; the centered text is only a layout choice.

## Fibonacci and FVG

Swing Fibonacci uses logarithmic prices. A 0.5 level is the geometric midpoint of the swing; the optional ratios are editable.

The optional FVG layer detects non-overlap between the first and third candles of a three-candle pattern. As price trades through a gap, MST trims or splits the remaining range. Visible boxes extend from the current bar, rather than from the gap's origin, and are filtered by the reference swing's display range.

## Display and settings

Auto colors adapt to light and dark chart backgrounds. Custom mode provides manual colors. The default layout emphasizes structure with selected Fibonacci levels; Cement markers and FVG fragments can be enabled in settings.

## Timing

Close mode selects the current close as the break source; it does not restrict evaluation to bar close. Open-bar results can change. Completed swing endpoints are drawn at the earlier extreme, and centered break labels do not represent an earlier signal. Working boundary arrows can differ from the reference boundaries used for break detection.

MST is open source under MPL-2.0.

## Disclaimer

I developed MST for my own chart analysis and share it as an open-source tool for education and independent market study. The indicator and its examples are not financial or investment advice, a recommendation, or an offer to buy or sell any instrument. Trading involves risk, including loss of capital. No accuracy, profitability or future result is guaranteed; historical examples do not predict future outcomes.
