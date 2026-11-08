# Changelog

## Unreleased

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
