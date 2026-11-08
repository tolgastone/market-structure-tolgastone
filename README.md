# Market Structure by tolgastone

Market Structure by tolgastone (MST) is an open-source TradingView indicator built around Cement: a close beyond preceding candle extremes. MST uses this rule to establish initial direction and confirm the completion of structural impulses, alongside boundary-break tracking, BOS/CHoCH, logarithmic Fibonacci and fair value gaps.

I developed MST for my own chart analysis and am sharing the approach as an open-source tool.

## How it works

Cement is the foundation of the approach. It provides a precise candle-based definition of confirmation rather than relying on candle body color. In the established structural cycle, boundary breaks and Cement have distinct roles:

A strict break of a reference swing boundary starts a structural impulse. The default source is Close; High/Low mode uses candle extremes. The engine tracks the impulse extreme until opposite-direction Cement confirms completion.

A bullish Cement closes above every high of the previous N bars; a bearish Cement closes below every low. Cement discovers the initial direction before a mature structural reference exists. An opposite boundary break can interrupt an impulse without Cement completion.

Fibonacci levels use logarithmic prices: 0.5 is a geometric midpoint. FVG tracking subtracts candle ranges from stored gaps and can trim or split them. Displayed FVG fragments are drawn from the current bar rather than their historical origin.

Read [The MST approach](docs/ALGORITHM.md) for the structural cycle, an example, and the implementation details behind the drawings.

**Current release:** [v1.0.0](https://github.com/tolgastone/market-structure-tolgastone/releases/tag/v1.0.0).

## Documentation

- [Illustrated concepts](docs/CONCEPTS.md): Cement candles, BOS, CHoCH and FVG.
- [Usage guide](docs/USAGE.md): setup, chart reading, timing and common questions.
- [Parameter reference](docs/PARAMETERS.md): all 33 controls, defaults and effects.
- [The MST approach](docs/ALGORITHM.md): structural logic and implementation details.
- [Chart examples](examples/README.md): light/dark defaults and optional layers.
- [Changelog](CHANGELOG.md): release notes.
- [Contributing](CONTRIBUTING.md): reporting issues and proposing changes.
- [TradingView description](docs/TRADINGVIEW_DESCRIPTION.md): publication text.

## Usage

Copy `ms_tolgastone.pine` into a new TradingView Pine script, save it, and add it to a standard candlestick chart. The script is self-contained and has no imported library or external-timeframe data requests. Add moving averages or other indicators separately if desired.

The default Auto palette adapts to light/dark chart backgrounds. The initial view emphasizes structure, with Cement markers and FVG fragments hidden and only the 0.5, 0.618 and 0.786 optional Fibonacci ratios enabled. Select Custom to use manual colors.

The on-chart title is **Market Structure by tolgastone**, with the short title **MST**.

## Timing and interpretation

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
