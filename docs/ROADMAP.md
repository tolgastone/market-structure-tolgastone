# Roadmap and research ideas

MST v1.2.1 is the current release, including the FVG display union fix. It adds opt-in structural break tolerance; None retains the published baseline. This document is the working backlog for future releases and separate experiments. It records direction, not a promise of delivery dates or a claim that the listed features already exist. Priorities and proposed versions can change as the design and evidence develop.

## Development principles

- Keep the published structural engine as the reference. Visual additions should use its existing swings rather than introduce a different structure model silently.
- Use the existing logarithmic Fibonacci geometry for OTE, Premium/Discount and Equilibrium. The 0.5 price is the geometric midpoint.
- Separate observations from engine decisions. A wick sweep marker need not change whether a structural break is accepted.
- Treat altered break thresholds, Cement completion rules and confirmation policies as behavior changes requiring explicit design and comparison.
- Validate experiments in a separate Lab before considering an optional product mode. Listing an experiment here does not authorize implementation or merging it into the published engine.

**Delivered:** v1.2.1 FVG Display Fix and v1.1.0 Swing Zones. See the [Swing Zones guide](SWING_ZONES.md). v1.2.0 adds optional Break Tolerance; BSL/SSL and sweep observations remain separate backlog items.

## Proposed release sequence

| Target | Focus | Proposed scope |
| --- | --- | --- |
| v1.1 — Swing Zones (delivered) | Shared Fibonacci visualization | OTE + Premium/Discount + Equilibrium |
| v1.2 — Break Tolerance (delivered) | User-selected structural acceptance | None / Percent / ATR and optional filtered-crossing markers; retain the baseline default |
| Liquidity — version to be assigned | Observable behavior around structural boundaries | BSL/SSL reference levels + same-bar wick/reclaim sweep markers |
| v1.3 — Alerts & Display | Event availability and display control | Structural alerts, FVG display improvements, drawing controls and input refinements |
| v2.0 candidates — Advanced Structure | Separate architectural assessment | EQH/EQL, MSS, Internal Structure and Order Blocks; scope and version not committed |

Bug fixes can be released independently of this sequence. A larger research result does not automatically belong in v2.0; its effect on the structural contract must be assessed first.

P1/P2/P3 below are backlog priorities, not references to the initial audit/cleanup phases. P1 does not mean that every item ships in the next release.

## P1 — additions using the current swing engine

| Feature | Intended behavior | Design boundary |
| --- | --- | --- |
| OTE Zone | Shade the active swing's logarithmic 0.618–0.786 region, with an optional 0.700 or 0.705 reference | Choose the reference ratio and visual controls before implementation; existing individual lines are not an OTE zone |
| Premium / Discount | Divide the active swing around its logarithmic 0.5 level | Prices above equilibrium are Premium; prices below are Discount, independent of swing direction |
| Equilibrium | Distinguish the logarithmic 0.5 level | Reuse the same level as Premium/Discount; avoid duplicate calculations and drawings |
| Liquidity Sweep | Mark a wick above a structural high or below a structural low followed by a close back inside on the same bar | Define the reference snapshot, equality rules, live/closed-bar availability and repeat handling; do not reclassify structural breaks automatically |
| BSL / SSL | Show potential buy-side/sell-side liquidity reference levels around structural highs/lows | These are inferred price references, not measurements of actual resting orders or proof of liquidity |
| FVG improvements | Simplify drawings; assess visibility, age and mitigation controls | Distinguish hiding old boxes, removing stored gaps and changing mitigation semantics; preserve a way to compare current fragment behavior |

These features can largely reuse the current swing engine. They still need precise definitions and checks; adding an overlay must not inadvertently change swing formation, event timing or reference selection.

### Suggested first implementation

Start with the shared Swing Zones layer. Define its working-swing anchors, ratio orientation, open-bar updates, colors, labels and object lifecycle together. A narrow first release would add these visualizations without changing any structural predicate or FVG lifecycle rule.

## P2/P3 — SMC and advanced structure

| Priority | Feature | Idea and open design questions |
| --- | --- | --- |
| P2 | EQH / EQL | Identify nearby highs/lows using a defined percentage or ATR tolerance; choose structural points, clustering, update and invalidation rules |
| P2 | Failed Close Break | Observe a close beyond HH/LL followed by a return inside the prior range within a defined number of bars; distinguish this from a same-bar wick sweep |
| P2 | MSS — Market Structure Shift | Identify an opposite smaller-scale break after a sweep; define the smaller structure and event ordering first |
| P2 | Internal Structure | Track smaller BOS/CHoCH events alongside the main structural swing, with independent state and clearly identified references |
| P2 | Displacement | Describe strong-bodied break candles relative to volatility; define body/range measure and normalization |
| P2 | Order Blocks | Identify candidate zones associated with structural moves; specify origin, qualification, bounds and invalidation |
| P3 | Breaker / Mitigation Blocks | Describe invalidated or revisited structural zones after their parent zone rules are established |
| P3 | Inverse FVG (IFVG) | Explore a possible role change after gap invalidation; define invalidation separately from current fragment consumption |

Internal Structure is an architectural task, not just another drawing switch. It needs its own scale definition, state transitions, event references, timing and resource budget. MSS and other features that depend on it should follow that design rather than assume a second structure already exists.

These feature names are working labels. They do not establish a single universal definition or imply predictive performance.

## Research pool — separate from the published engine

Percent minimum penetration and ATR thresholds have been selected for an opt-in v1.2.0 product option; see [Break tolerance](BREAK_TOLERANCE.md). Other experiments below remain separate research and are not scheduled for implementation.

### Gate A — structural break acceptance

The baseline (None) accepts a strict boundary crossing using Close or High/Low. A small excess is still a break. The working source adds selectable Percent and ATR distances without changing that default; other acceptance alternatives remain research.

| Experiment | Candidate rule | Decisions needed |
| --- | --- | --- |
| Minimum penetration | Percent distance implemented in v1.2.0 | Strict boundary-relative distance, no tick rounding; tested sub-threshold crossings can be marked T |
| ATR threshold | ATR distance implemented in v1.2.0 | Previous closed-bar ATR, refreshed each bar; no per-swing freeze |
| Swing-range threshold | Require an excess proportional to the reference swing range, e.g. an illustrative 33% | Exact range definition, logarithmic geometry where appropriate, anchoring and parameter bounds; 33% is not an endorsed default |
| Volume confirmation | Evaluate a break against relative volume | Data availability, baseline window and the behavior when volume is missing or unreliable |
| Break & Retest | Treat the initial break as a candidate and wait for acceptance/retest | Retest bounds, timeout, invalidation and event availability |
| Follow-up confirmation | Keep the first small break provisional until a later candle confirms it | Candidate state, number/type of candles, timeout and what invalidates the candidate |

Rejecting a small break does not automatically classify it as a liquidity sweep. These rules also affect references and later swings, not only the label on one candle. Specify whether a candidate holds the old reference and how new extremes are treated while it waits.

### Gate B — impulse completion / Cement confirmation

The current default is one opposite Cement with lookback 1. The existing lookback N compares one close with every high/low of the preceding N bars. It is not a count of confirmation candles and its window is not reset at the start of an impulse.

| Experiment | Candidate rule | Decisions needed |
| --- | --- | --- |
| B1 — Two Cement | Complete on a second opposite-direction Cement | Whether intervening non-Cement bars are allowed; candidate reset, expiry and new-extreme handling |
| Consecutive Cement | Require consecutive opposite-direction Cement candles | Consecutive means adjacent chart bars; define reset and availability |
| First-Cement boundary confirmation | First opposite Cement creates a candidate; a later candle confirms beyond a selected boundary of that first Cement | High/low versus close reference, whether the second candle must itself be Cement, intervening bars and invalidation |
| B2 — Cement + Pivot | Seek a pivot confirmation after Cement | Pivot rule, future-bar requirement and explicit added confirmation delay |
| B3 — Retracement confirmation | Require a defined retracement, e.g. an illustrative 0.236 | Working/reference anchors, logarithmic level, source price and re-anchoring behavior |
| B4 — ATR correction | Require an opposite movement of a minimum ATR size | Measurement anchor, volatility sample, threshold and invalidation |

More confirmation is not automatically more accurate structure. It can merge swings, delay completion, change bootstrap direction and alter the subsequent BOS/CHoCH chain. Research must compare these consequences rather than describe an alternative as an improvement by assumption.

### Evidence required before adopting a research option

- Write the candidate, confirmation, timeout and invalidation rules before implementation.
- Use the same chart data and initial history when comparing with the published baseline.
- Record event time separately from drawn extreme time, with both directions, equal touches, outside bars and interruptions represented.
- Measure confirmation delay, changed/merged swings, reference changes and resource cost; do not evaluate only visually attractive examples.
- Review open-bar, closed-bar, replay and reload behavior explicitly. A fixed pivot window, if introduced, must be disclosed.
- Promote an experiment only after an explicit product decision. Keep the original mode available where compatibility is intended.

## Product quality and supporting work

| Area | Direction |
| --- | --- |
| Structural Alerts | Separate bullish/bearish BOS and CHoCH events; specify reference, direction, availability and duplicate suppression |
| Cement Alerts | Decide whether alerts mean every qualifying Cement candle or only structural completion markers |
| Liquidity Alerts | Follow the agreed sweep rules; add EQH/EQL interactions only after those levels are defined |
| Confirmation Policy | Distinguish provisional open-bar observations from closed-bar results; do not silently change the current realtime policy |
| Visual Presets | Explore Minimal / Standard / Detailed layouts without confusing them with the existing Auto/Custom color mode |
| Drawing Controls | Manage retained swings, FVGs, labels and other objects; distinguish display retention from computational storage |
| FVG resources | Assess long-history growth and repeated splitting before choosing caps or pruning rules |
| Reliability | Cover short-history startup, price-domain assumptions, both break sources and Cement lookback 1–10 |
| Settings Documentation | Keep the existing parameter guide synchronized with every added or changed setting |

Auto/Custom theme colors, input help, and the algorithm/usage/parameter guides already exist in v1.0.0. Future work extends them rather than treating them as missing features.

## Updating this roadmap

Before starting a feature, select one item and turn it into a small scope with acceptance criteria. On completion, move its delivered behavior into the changelog and relevant guides. Keep unresolved alternatives in this roadmap rather than documenting them as implemented functionality.

See the [current algorithm](ALGORITHM.md), [parameters](PARAMETERS.md), [changelog](../CHANGELOG.md) and [contribution guide](../CONTRIBUTING.md) for the published behavior and change process.
