# Changelog

## Unreleased

## 1.2.2 — 2026-11-08

### Changed

- Reuse FVG display boxes across latest-bar updates; update coordinates/colors, create missing boxes and delete surplus slots. Same-direction union, stored-fragment lifecycle and logarithmic visibility range are unchanged.

- Reuse classic Fibonacci line/label pairs, update their coordinates/text/colors, and delete surplus pairs when the visible level count falls. Recreate handles removed by platform garbage collection. Logarithmic levels, label formatting and zone-based level suppression are unchanged.

- Reuse the two working-swing boundary arrows instead of deleting and recreating them on each latest-bar update. Recreate a handle if platform garbage collection removed it; coordinates, color and arrow style are unchanged.

## 1.2.1 — 2026-11-08

### Changed

- Merge overlapping or touching same-direction FVG fragments into one display region to avoid stacked opacity. Opposite directions retain separate colors.
- Preserve stored fragments, mitigation rules, full-fragment range eligibility and the reference-swing logarithmic display range.

## 1.2.0 — 2026-11-08

### Added

- Optional None / Percent / ATR structural break tolerance for normal breaks and opposite-boundary interruptions. None and zero-distance settings retain the preceding behavior.
- Previous closed-bar ATR sampling with configurable period/multiplier; distances refresh each bar.
- Optional T markers for tested crossings rejected by tolerance, independently of structural drawings. No liquidity-sweep classification is applied.
- Break-tolerance guide and updated parameter, usage and algorithm documentation. Cement rules, FVG lifecycle and logarithmic zone geometry are unchanged.

## 1.1.0 — 2026-11-08

### Added

- Working-swing OTE, Premium/Discount and Equilibrium layers using logarithmic prices.
- Independent visibility, Auto/Custom colors and optional 0.700/0.705 OTE reference.
- Bounded reusable drawings and suppression of matching classic Fibonacci annotations.
- Structural engine, Cement, break rules and FVG maintenance unchanged.

### Documentation

- Add a roadmap separating proposed product releases, quality work and structural-engine research.

- Explain logarithmic Fibonacci and FVG display-range geometry, including chart-axis independence and the distinction from direct OHLC comparisons.
- Clarify realtime evaluation, variable Cement confirmation and the absence of a fixed right-hand fractal/pivot window; open-bar results remain provisional.

## 1.0.0 — 2026-11-08

### Initial release

- Cement-based market structure with tracked swing boundaries and impulse extremes.
- BOS and CHoCH annotations for structural breaks.
- Logarithmic swing Fibonacci levels with editable ratios.
- Fair value gaps with remaining-range tracking.
- Automatic light/dark colors and a customizable palette.
- Optional Cement markers, swing points, boundary arrows and FVG display.
- English documentation, illustrated concepts and chart examples.

Licensed under MPL-2.0.
