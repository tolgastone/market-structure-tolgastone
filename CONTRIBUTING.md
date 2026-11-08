# Contributing

Contributions should make MST easier to understand, use and maintain while preserving its structural approach. Read [The MST approach](docs/ALGORITHM.md) and the [parameter reference](docs/PARAMETERS.md) before proposing changes.

## Source layout

- `ms_tolgastone.pine` is the only maintained Pine source. Edit it directly; do not add parallel source copies or generated variants.
- `docs/` contains algorithm, usage and parameter documentation.
- `examples/` contains chart images and their configuration notes.
- `CHANGELOG.md` records user-visible changes.

## Reporting a problem

Provide the symbol and exchange/feed, chart type, interval, chart timezone, approximate loaded history, relevant dates, all changed inputs and the source revision. Include exact compiler/runtime messages if applicable and a focused chart image. State expected behavior, observed behavior and reproduction steps.

For timing reports, distinguish an open-bar observation from a closed-bar result. Record the event bar separately from its drawn endpoint or centered label. Specify whether the observation came from live updates, replay or a reload.

## Proposing a change

Keep each change focused. Separate visual/documentation edits, performance work and changes to structural or FVG semantics. Describe the behavior before and after the change and how it was checked. Discuss changes to the algorithm or confirmation policy before implementing them.

Preserve these distinctions unless an explicit behavior change is agreed:

- Normal impulses begin at strict reference-boundary breaks; opposite Cement normally confirms completion.
- Initial direction uses Cement bootstrap, while an opposite-boundary interruption can complete an impulse without Cement.
- State blocks, up/down break tests and candidate-extreme updates have deliberate execution order.
- Fibonacci uses log prices; FVG maintenance can trim and split fragments.

Do not bundle unrelated trading systems or experimental research into a maintenance change.

## Validation

Compile the changed source in TradingView Pine v6. Compare with the preceding source on identical symbol/feed, chart type, interval, history and retained input values.

- Visual changes: review both light and dark backgrounds, Auto and Custom colors, and affected visibility switches.
- Structural changes: compare event type/direction/bar and swing prices/indices; cover continuation/reversal, both break sources, Cement lookback extremes, equal touches, outside bars and interruptions as applicable.
- FVG changes: check creation, overlap/removal, trimming, splitting, equality touches and display-off behavior.
- Performance changes: measure relevant resource behavior and show that retained outputs still match.

Report untested cases and limitations. Compilation and a static screenshot alone do not demonstrate live timing parity.

## Documentation and licensing

Use English for public documentation and comments. Update affected guides and the Unreleased changelog. Preserve the MPL-2.0 notice and existing attribution. Identify the source and compatible license of any proposed third-party code.

Keep submitted files limited to product source, documentation and reproducible examples. Exclude credentials, personal account details, local workspace settings and private development records.
