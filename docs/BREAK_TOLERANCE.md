# Structural break tolerance (v1.2.0)

Break tolerance optionally changes which reference-boundary crossings MST accepts. It changes swing formation and the later BOS/CHoCH chain; it is not just a label filter. Introduced in v1.2.0; v1.1.0 has no tolerance setting.

The default **None** preserves the preceding engine. Cement initialization, lookback and completion predicates are unchanged. No liquidity-sweep classification, BSL/SSL layer, retest, multi-candle confirmation or closed-bar-only policy is added.

## Acceptance rules

Break source remains independent: Close uses the current close for both directions; High/Low uses high for upward tests and low for downward tests. Let H/L be the reference swing boundaries, p the percentage input divided by 100, A the previous closed bar's ATR and k its multiplier.

| Mode | Upward break | Downward break |
| --- | --- | --- |
| None | source > H | source < L |
| Percent | source > H + abs(H) × p | source < L − abs(L) × p |
| ATR | source > H + A × k | source < L − A × k |

Comparisons are strict. Equality at either the original boundary or the tolerance threshold is not an accepted break. Thresholds use full calculation precision without tick rounding. Percentage is an ordinary boundary-relative percentage, not a logarithmic interpolation or a percentage of swing range. Fibonacci, Swing Zones and the FVG display range retain their logarithmic geometry.

For a reference low of 60,000, Percent = 0.5 requires a source below 59,700; exactly 59,700 is insufficient. A source of 59,577 passes 0.5% but does not pass 1%, whose threshold is 59,400. These values illustrate the rule, not recommended trading settings.

## ATR sampling

ATR uses TradingView's built-in `ta.atr(length)` (Wilder smoothing) on the chart timeframe. The distance uses its **previous bar** value: `ta.atr(length)[1] × multiplier`. Therefore the current break candle does not inflate its own distance and the ATR sample stays constant through that open bar.

The distance is recalculated each bar; it is **not frozen for the lifetime of the reference swing**. A smaller ATR on a later bar can permit a break at a smaller price excess. Acceptance checks the current price against the current threshold; it does not require a fresh crossover of the original boundary.

With a positive multiplier, structural breaks wait until the previous-bar ATR is available. Cement bootstrap and normal Cement completion still run during warm-up. No T marker is emitted solely because ATR is unavailable. A zero percentage or zero ATR multiplier disables the filter and retains the original behavior, including during ATR warm-up.

## Where the filter applies

The same tolerance applies to normal upward/downward breaks and opposite-boundary interruptions during an active impulse. A rejected excursion does not start or interrupt an impulse through that break path. Normal extreme tracking, candidate updates and Cement completion continue according to the current state. Thus the filter does not promise that a marked candle cannot also complete an existing swing through Cement.

The original independent state-block ordering and upward/downward check order are retained. In High/Low mode, an outside bar can still pass both normal break checks. Filtering can delay or remove a break and consequently change anchors, references and all subsequent swings.

For an accepted interruption in a nonzero tolerance mode, the drawing keeps the direction accepted before the reference is replaced. None and zero-distance modes retain the preceding drawing behavior.

## Optional T markers

Enable **Filtered break markers (T)** to mark source prices beyond the original reference boundary but not beyond its tolerance threshold. The default is Off.

- T above a bar: a tested upward crossing was filtered.
- T below a bar: a tested downward crossing was filtered.
- Only boundary tests made by the structural engine are observed. During an active impulse, this is the opposite boundary; in the normal waiting-for-break path, both boundaries are tested.
- A direction is not marked when an accepted break in that direction also occurred during the same execution. Both directions may be marked on one outside bar.
- Repeated qualifying bars may each receive a mark. No candidate state, expiry or per-level suppression is introduced.
- Close mode observes Close; an isolated wick beyond a level with a close inside does not produce T. High/Low mode observes the extremes.
- Marks use the theme-aware neutral Equilibrium color. Hiding them does not change calculations; they do not consume historical label objects.

T means a filtered crossing, not a liquidity sweep or a trade signal. Open-bar marks are provisional and may disappear or become accepted breaks before close. No closed-bar guard is added.

Working boundary arrows still show the working swing's original high and low. They are not tolerance-threshold lines; dashed BOS/CHoCH lines remain at the original structural reference boundary.

## Compare before publishing

Compile in Pine v6 and compare None against v1.1.0 on identical data, history and inputs. Check both directions and sources, Cement lookbacks 1 and 10, threshold equality, outside bars, interruptions, ATR warm-up, changing ATR, open-bar/reload behavior and marker visibility. Synthetic offline comparisons do not certify TradingView compilation or tick-by-tick execution.
