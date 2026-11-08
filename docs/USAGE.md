# Using Market Structure by tolgastone

MST displays structural swings, boundary breaks, Cement markers, logarithmic Fibonacci levels, and surviving fair value gap fragments. Its short chart title is **MST**.

## Add the script

1. Open a standard candlestick chart in TradingView.
2. Copy the complete contents of [`ms_tolgastone.pine`](../ms_tolgastone.pine) into a new Pine Editor indicator script.
3. Save the script and add it to the chart. No separate library is required.
4. Start with **Break source = Close**, **Break tolerance = None** and **Cement lookback bars = 1**, the current defaults.
5. Leave **Color mode = Auto** to follow the chart background, or select **Custom** to use the color controls. The default view shows swings, extreme markers, breaks, boundaries and Swing Zones. EQ and OTE provide the dedicated 0.5 and 0.618–0.786 annotations; matching classic Fibonacci labels are omitted while these layers are enabled. Cement markers and FVG fragments start hidden. Open the indicator settings to choose additional layers. See the [parameter reference](PARAMETERS.md) for every control.

The same Auto palette adapts to light and dark chart backgrounds; there are no separate Light/Dark presets.

There is no timeframe input: calculations use the chart's bars. Changing the chart interval changes what each bar, boundary break and Cement comparison represents. Use positive-price data for logarithmic calculations.

## Read the structure first

The main sequence is **reference boundary break → impulse extreme tracking → opposite-Cement completion**. Initial direction and opposite-boundary interruption are exceptions described in [The MST approach](ALGORITHM.md).

| Drawing | Meaning |
| --- | --- |
| Connecting swing lines | Structural legs drawn when swings complete or are interrupted; continuation can also draw an intervening connecting leg |
| High / low circles | High / low endpoints of drawn swings |
| BOS dashed line and label | Normal-path break in the reference swing's direction |
| CHoCH dashed line and label | Normal-path break against the reference swing's direction |
| Bullish / bearish triangles | Bullish / bearish Cement markers emitted by the structural engine; these do not mark every candle satisfying the Cement predicate |
| T above / below a bar (optional, v1.2.0) | A tested upward / downward source crossing filtered by the selected tolerance; not a sweep classification |
| Two right-pointing boundary arrows | Working swing high and low, not necessarily the reference thresholds used to detect breaks |

Auto uses theme-aware teal/coral directional colors for BOS/CHoCH lines and text, extreme circles and Cement markers. In Custom mode, Bullish color and Bearish color control these elements, while Structure line color affects swing lines. Structure line width affects swing lines; dashed break lines retain their fixed width.

For a less crowded view, keep swings, break labels and boundary arrows visible and turn off **Show Fibonacci levels** and **Show FVG fragments**. This is an optional viewing arrangement, not a change to the shipped defaults. Historical swing lines have no master visibility switch.

## Adjust break sensitivity (v1.2.0)

Keep **Break tolerance = None** for the preceding behavior. Percent requires a boundary-relative price excess; ATR requires an excess based on the previous closed bar's ATR. The same filter applies to normal breaks and opposite-boundary interruptions. Nonzero settings can change the entire swing chain; Cement completion remains active.

Enable **Filtered break markers (T)** to see tested sub-threshold crossings. T follows the selected Close or High/Low source, can repeat on later bars, and can change before close. Working arrows do not show tolerance thresholds. See [Break tolerance](BREAK_TOLERANCE.md) for examples and ATR warm-up behavior.

## Understand Cement

See the [illustrated concepts](CONCEPTS.md) for candle examples and explanations of BOS, CHoCH and FVG.

With lookback 1, bullish Cement means the current close is strictly above the previous bar's high; bearish Cement means it is strictly below the previous bar's low. With a larger lookback, the close must exceed every high or fall below every low in that preceding window.

Lookback 3 does not mean three consecutive Cement candles. An UP impulse normally completes on bearish Cement; a DOWN impulse normally completes on bullish Cement. Hiding markers does not disable this logic.

## Swing Zones (v1.1.0)

The new [Swing Zones layer](SWING_ZONES.md) adds independent OTE, Premium/Discount and Equilibrium displays. It follows the working swing and can change during an open bar. Zones remain visible independently of classic Fibonacci. Matching classic ratio annotations are replaced while the dedicated layers are enabled. These controls were introduced in v1.1.0.

## Add Fibonacci context

Enable **Show Fibonacci levels** to display the working swing's logarithmic ratios. Each Enable / Ratio pair controls one optional level. Ratios 0 and 1 are always included while the layer is visible.

The levels use log prices, so 0.5 is the geometric midpoint rather than the arithmetic midpoint. For an UP swing, ratio 0 is the high and ratio 1 is the low; for a DOWN swing, ratio 0 is the low and ratio 1 is the high. Ratios below 0 and above 1 extend beyond the swing range.

The classic numeric 0.618 and 0.786 lines are individual levels; the dedicated OTE band is provided separately by Swing Zones in v1.1.0. **Fibonacci color** and **Boundary color** are independent controls in Custom mode. Auto supplies separate muted colors for both layers.

The logarithmic formulas remain in use whether the chart price axis is linear or logarithmic; there is no linear-Fibonacci mode. For example, a 100–200 swing has its 0.5 level near 141.42, not 150. The FVG display-range filter also uses log-price geometry. This does not change the direct OHLC comparisons used for breaks, Cement or gap maintenance.

## Read FVG fragments

FVGs are detected using the current bar and the bar two periods earlier. Later candle ranges can remove, trim or split a stored gap. A first touch does not automatically remove the entire gap.

Visible boxes show surviving fragments inside the reference swing's display range. They start at the current bar and extend 30 bars to the right; they do not identify the original gap candle. A missing box can mean a fragment is outside the display range, not that every stored gap has been filled.

v1.2.1 draws overlapping/touching same-direction fragments as a single region. A positive price gap remains visible, and opposite directions keep separate colors. The merge changes only the drawing; each stored fragment continues its existing lifecycle.

Turning **Show FVG fragments** off hides the boxes but leaves tracking active. The two fragment color controls change appearance only.

## Separate chart position from availability

### Live evaluation and confirmation

MST evaluates structure on each realtime update without waiting for a fixed number of right-hand candles to confirm a fractal pivot. Impulse completion follows opposite-direction Cement, so confirmation time depends on price action rather than a preset future-bar count.

Open-bar results remain provisional and can change before close. Completed swings are drawn back to their tracked extremes; those endpoints were not confirmed at the earlier extreme bar. This is not a claim of zero delay or non-repainting behavior.

On an open bar, Close is the changing current price. Breaks, Cement predicates, FVGs and drawings can change before the bar closes. The script does not enforce close-only confirmation.

A swing drawn at an earlier extreme becomes visible only when the engine draws it on a later completion or interruption. A BOS/CHoCH label is centered along its dashed line; use the line's right endpoint to identify its break bar. Do not treat label position as an earlier signal.

For a meaningful before/after comparison, keep the symbol/feed, chart type, interval, loaded history and retained input values the same. Capture closed-bar examples separately from open-bar observations. Historical replay alone does not establish tick-by-tick live behavior.

## Common questions

**Why are the boundary arrows different from a break level?** They follow the working swing, while break tests use the reference swing.

**Why do early structures differ after loading more history?** Initialization starts from the first loaded bar and the first Cement. A different starting history can produce a different early state.

**Why are some older drawings missing?** Line, label and box budgets limit the displayed objects. Visible history is not a complete event log.

**Can I create a dedicated MST BOS/CHoCH alert?** The current source does not define custom structural alert conditions.

**Can I combine MST with other indicators?** Yes. Add any moving averages or other tools separately; they do not form part of MST's structural engine.

## Chart title and Fibonacci contrast

Input values are hidden from the chart status line so the short title remains MST; all controls remain available in settings. Numeric/string inputs remain visible in the Data Window. In Auto mode on a dark background, Fibonacci text uses a brighter slate tone while the lines retain their subdued color. Light-background and Custom-mode Fibonacci colors are unchanged.

## Intended use

MST is an analytical tool, not a trading instruction or investment recommendation. See the [disclaimer](../DISCLAIMER.md).
