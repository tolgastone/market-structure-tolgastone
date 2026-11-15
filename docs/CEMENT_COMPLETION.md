# Impulse completion modes (v1.3.0)

v1.3.0 offers three selectable completion policies. **Single Cement** is the default and retains the published behavior. No alternative is claimed to produce better structure. These options can change completion time, swing anchors, later references and the entire BOS/CHoCH chain.

## Rules

| Impulse completion | UP impulse | DOWN impulse |
| --- | --- | --- |
| Single Cement | Complete on first bearish Cement | Complete on first bullish Cement |
| Two Cements | Complete on second bearish Cement since the last candidate reset; intervening bars are allowed | Complete on second bullish Cement since the last reset; intervening bars are allowed |
| First Cement Boundary | First bearish Cement seeds a candidate; a later close strictly below its wick low confirms completion | First bullish Cement seeds a candidate; a later close strictly above its wick high confirms completion |

Cement lookback remains a separate input. N means one close beyond every high/low of the preceding N bars; it does not mean N Cement events. Both count-mode events use the selected N. In boundary mode only the first candidate must be Cement; the later confirmation need not be.

Boundary confirmation always uses Close and the original candidate's wick low/high. It is independent of the structural Break source and None/Percent/ATR tolerance. Equality and wick-only crossing do not confirm it. A candidate cannot confirm on the same chart bar that created it.

## Candidate lifecycle

- A strictly new impulse high (UP) or low (DOWN) clears the candidate and count before evaluating completion. An equal high/low does not reset.
- The new-extreme bar can itself seed a fresh candidate if it is an opposite Cement. Single Cement can still complete on a new-extreme bar, matching the published update-before-completion ordering.
- Non-Cement intervening bars do not clear a candidate. Neither does a same-direction Cement unless it also creates a new extreme. There is no time expiry.
- The first candidate's boundary stays fixed while it is pending. Later Cements do not replace it in boundary mode.
- Entering the structural break stage clears candidate/count, including after completion or an accepted opposite-boundary interruption. A new impulse starts with no inherited candidate.

Initial direction still uses the existing Cement bootstrap. Opposite structural boundary breaks retain priority and can interrupt an impulse before any completion-policy confirmation, using the selected structural source/tolerance.

## Same candles, different timing

Assume an UP impulse already reached high 110; the preceding candle low is 106. No opposite structural interruption occurs. Lookback is 1. Prices are illustrative.

| Bar | High | Low | Close | Observation |
| --- | --- | --- | --- | --- |
| A | 109 | 103 | 104 | First bearish Cement: 104 < 106 |
| B | 108 | 100 | 105 | Not bearish Cement; wick crosses the candidate low but close does not |
| C | 107 | 99 | 102 | Not bearish Cement because 102 is above B low 100; closes below candidate low 103 |
| D | 103 | 97 | 98 | Second bearish Cement: 98 < 99 |

Single Cement completes at A, First Cement Boundary at C, Two Cements at D. All retain the same extreme at 110 in this sequence. If a new high occurs while an alternative is waiting, that impulse can extend and its candidate resets; the resulting structural segmentation can then differ from Single Cement.

Two Cements is not always later than boundary confirmation. After A, a bar with low 106 followed by a close of 105 creates a second bearish Cement while remaining above A low 103: count mode can confirm before boundary mode.

## Timing and display

All modes retain realtime evaluation with standard Pine rollback; no closed-bar-only guard or intrabar-persistent counter is added. Open-bar candidates and completion results may change before close. Repeated updates of one bar are not separate stored Cement events. Historical completion endpoints remain drawn at their tracked extreme bars rather than at the confirmation bar.

The existing Cement markers switch now represents the selected policy's completion marker. In First Cement Boundary mode the marked confirmation candle need not itself be Cement. Candidate bars receive no additional marker. Hiding markers does not disable the policy.

FVG tracking, logarithmic Fibonacci and Swing Zones formulas are unchanged, but display anchors/ranges can change when the selected policy changes the swings.

## Validation and future alternatives

Compare on identical symbol, timeframe, loaded history and other inputs. Begin with Single Cement parity, then both directions, interrupted impulses, candidate resets, equal extremes, lookbacks 1/10 and open-bar/reload observations. Offline synthetic checks do not replace Pine v6 compilation and chart validation.

A future fractal-only approach would be a separate alternative, not part of these Cement modes. Its left/right windows, confirmation delay, initialization and interaction with structural breaks must be designed independently. No fractal detector is added in v1.3.0.
