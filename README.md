# Market Structure by tolgastone

[Use MST on TradingView](https://www.tradingview.com/script/7TyceMq5-Market-Structure-by-tolgastone/) · [Illustrated guide](docs/CONCEPTS.md)

Market Structure by tolgastone (MST) is an open-source TradingView indicator built around Cement: a close beyond preceding candle extremes. MST uses this rule to establish initial direction and confirm the completion of structural impulses, alongside boundary-break tracking, BOS/CHoCH, logarithmic Fibonacci and fair value gaps.

I developed MST for my own chart analysis and am sharing the approach as an open-source tool.

## How it works

Cement is the foundation of the approach. It provides a precise candle-based definition of confirmation rather than relying on candle body color. In the established structural cycle, boundary breaks and Cement have distinct roles:

A strict break of a reference swing boundary starts a structural impulse. The default source is Close; High/Low mode uses candle extremes. The engine tracks the impulse extreme until opposite-direction Cement confirms completion.

A bullish Cement closes above every high of the previous N bars; a bearish Cement closes below every low. Cement discovers the initial direction before a mature structural reference exists. An opposite boundary break can interrupt an impulse without Cement completion.

Fibonacci levels use logarithmic prices: 0.5 is a geometric midpoint. FVG tracking subtracts candle ranges from stored gaps and can trim or split them. Displayed FVG fragments are drawn from the current bar rather than their historical origin.

Read [The MST approach](docs/ALGORITHM.md) for the structural cycle, an example, and the implementation details behind the drawings.

**Current release:** [v1.0.0](https://github.com/tolgastone/market-structure-tolgastone/releases/tag/v1.0.0).

## Logarithmic price geometry

MST uses logarithmic prices for swing Fibonacci levels and for the expanded reference-swing range that filters visible FVG fragments. Distances in log space represent price ratios rather than equal absolute price differences. There is no linear-Fibonacci calculation mode.

For a swing from 100 to 200, the 0.5 level is `sqrt(100 * 200)`, approximately **141.42**, rather than the arithmetic midpoint of 150. The expanded FVG display range extends one log-swing distance below the low and above the high.

The chart's linear/log price-axis setting does not change these formulas. All prices used in logarithmic calculations must be positive.

Boundary breaks, Cement predicates, and FVG creation/trimming still compare actual OHLC prices directly. Logarithmic geometry does not replace those structural rules or make FVG detection a logarithmic interpolation.

**Next release candidate:** [v1.1 Swing Zones](docs/SWING_ZONES.md) adds OTE, Premium/Discount and Equilibrium as a display-only layer. Compilation and reviewed light/dark examples passed; the current published release remains v1.0.0.

## Documentation

- [Illustrated concepts](docs/CONCEPTS.md): Cement candles, BOS, CHoCH and FVG.
- [Usage guide](docs/USAGE.md): setup, chart reading, timing and common questions.
- [Parameter reference](docs/PARAMETERS.md): published settings and the v1.1 development controls, defaults and effects.
- [The MST approach](docs/ALGORITHM.md): structural logic and implementation details.
- [Chart examples](examples/README.md): light/dark defaults and optional layers.
- [Roadmap and research ideas](docs/ROADMAP.md): proposed releases, priorities and separate experiments.
- [Changelog](CHANGELOG.md): release notes.
- [Contributing](CONTRIBUTING.md): reporting issues and proposing changes.
- [TradingView description](docs/TRADINGVIEW_DESCRIPTION.md): publication text.

## Usage

Copy `ms_tolgastone.pine` into a new TradingView Pine script, save it, and add it to a standard candlestick chart. The script is self-contained and has no imported library or external-timeframe data requests. Add moving averages or other indicators separately if desired.

The default Auto palette adapts to light/dark chart backgrounds. The initial view emphasizes structure, with Cement markers and FVG fragments hidden and only the 0.5, 0.618 and 0.786 optional Fibonacci ratios enabled. Select Custom to use manual colors.

The on-chart title is **Market Structure by tolgastone**, with the short title **MST**.

## Timing and interpretation

### Live structure without a fixed pivot window

MST evaluates structure on each realtime update without waiting for a fixed number of right-hand candles to confirm a fractal pivot. Impulse completion follows opposite-direction Cement, so confirmation time depends on price action rather than a preset future-bar count.

Open-bar results remain provisional and can change before close. Completed swings are drawn back to their tracked extremes; those endpoints were not confirmed at the earlier extreme bar. This is not a claim of zero delay or non-repainting behavior.

- Close mode selects the price source; it does not restrict evaluation to bar close. Open-bar breaks, Cement markers, and FVGs can change before close.
- Completed swing endpoints can be drawn on earlier extreme bars. A historical endpoint or centered BOS/CHoCH label does not show when the event first became available.
- Current boundary arrows follow the working swing; break detection uses the reference swing.
- Loaded history affects initialization. Drawing limits can remove older objects.
- Logarithmic calculations assume positive prices.

## Source layout

`ms_tolgastone.pine` is the sole Pine source. Guides are in `docs/`; chart images and configuration notes are in `examples/`.

## Disclaimer

For educational and informational use only; not financial or investment advice. Trading involves risk, and no result is guaranteed. See the [full disclaimer](DISCLAIMER.md).

## License

The Pine source is licensed under the [Mozilla Public License 2.0](LICENSE). Copyright © tolgastone.
