# Roadmap and research ideas

MST v1.2.2 is the current release, including drawing reuse and the FVG display union fix. It adds opt-in structural break tolerance; None retains the published baseline. This document is the working backlog for future releases and separate experiments. It records direction, not a promise of delivery dates or a claim that the listed features already exist. Priorities and proposed versions can change as the design and evidence develop.

## Development principles

- Keep the published structural engine as the reference. Visual additions should use its existing swings rather than introduce a different structure model silently.
- Use the existing logarithmic Fibonacci geometry for OTE, Premium/Discount and Equilibrium. The 0.5 price is the geometric midpoint.
- Separate observations from engine decisions. A wick sweep marker need not change whether a structural break is accepted.
- Treat altered break thresholds, Cement completion rules and confirmation policies as behavior changes requiring explicit design and comparison.
- Validate experiments in a separate Lab before considering an optional product mode. Listing an experiment here does not authorize implementation or merging it into the published engine.

## Delivered releases

| Release | Completed scope |
| --- | --- |
| v1.0.0 | Cement-based structural engine, BOS/CHoCH, logarithmic Fibonacci, FVG tracking, theme-aware display and illustrated documentation |
| v1.1.0 — Swing Zones | OTE 0.618–0.786, optional 0.700/0.705 reference, Premium/Discount and geometric Equilibrium |
| v1.2.0 — Break Tolerance | None / Percent / ATR structural acceptance, previous closed-bar ATR sampling and optional T markers; None retains baseline behavior |
| v1.2.1 — FVG Display Fix | Display-only union of overlapping/touching same-direction fragments; stored-gap lifecycle and visibility range retained |
| v1.2.2 — Drawing Reuse | Reuse working boundary arrows, classic Fibonacci line/label pairs and FVG boxes; remove surplus objects and recreate missing handles |

FVG display union and drawing reuse are complete. FVG age/count/mitigation controls and long-history storage optimization are separate, unfinished items. Reusing boxes does not reduce stored fragment history.

See the [changelog](../CHANGELOG.md), [Swing Zones](SWING_ZONES.md) and [Break tolerance](BREAK_TOLERANCE.md) for delivered behavior.

## Current work order

This order replaces the earlier release sketch that put Alerts & Display next. Future version numbers will be assigned after scope and compatibility are established; there is no committed v1.3 or v2.0 feature bundle.

| Order | Workstream | Status and next boundary |
| --- | --- | --- |
| 1 — next design task | Cement completion options | Define and illustrate the existing single opposite Cement, two opposite Cements with intervening bars, consecutive opposite Cements, and confirmation beyond the first opposite Cement's boundary. Design and comparison first; no new completion mode is implemented or selected for release. |
| 2 | Separate Cement experiment | After rules are agreed, compare a selected candidate with the published baseline in a separate Lab. Assess delay, new extremes, interruption and the subsequent swing chain before deciding whether to offer an optional product mode. |
| 3 | Boundary observations and quality work | BSL/SSL and sweep overlays remain candidates, alongside drawing controls and measured FVG storage/maintenance work. Select a narrow task based on observed need; these are not prerequisites for Cement research. |
| 4 | Advanced structure | Assess EQH/EQL, Internal Structure, MSS, Displacement and Order Blocks individually. Dependencies and exact definitions come before implementation. |
| Last | Alerts | Deferred. OTE contact is the first alert priority when this workstream is opened; structural, Cement and liquidity alerts follow later. |

Documentation is maintained alongside each workstream: record completed behavior, update the next task and keep unresolved ideas separate. Bug fixes and measured maintenance can interrupt this order when needed.

### Acceptance criteria for the next Cement design

- Use the existing one-opposite-Cement rule as the comparison baseline; Cement lookback N remains distinct from a count of confirmation candles.
- Illustrate both UP and DOWN impulses with the extreme bar, first candidate bar and completion bar identified separately.
- Define intervening-bar rules, candidate reset/expiry, and what happens if a new extreme appears while confirmation is pending.
- Define interaction with opposite-boundary interruptions and None / Percent / ATR break tolerance.
- Specify whether a proposal affects initialization, impulse completion or both, and distinguish provisional open-bar observations from closed-bar results.
- Compare alternatives before selecting one. No default-engine change follows merely from listing or illustrating a candidate.

## Overlay and quality backlog

| Item | State | Remaining scope |
| --- | --- | --- |
| OTE / Premium-Discount / Equilibrium | Delivered v1.1.0 | Maintain logarithmic geometry and independent display controls |
| FVG visual simplification | Delivered v1.2.1–v1.2.2 | Same-direction union and object reuse complete; additional lifecycle/storage controls are not implemented |
| BSL / SSL | Backlog | Define structural references and level lifecycle. These are inferred price references, not measurements of resting orders. |
| Liquidity Sweep | Backlog | Define wick/reclaim references, equality, availability and repeats. Keep separate from T markers and structural acceptance. |
| Drawing controls / presets | Backlog | Define historical retention and Minimal / Standard / Detailed views without changing calculations silently |
| FVG resources and controls | Investigation pending | Measure growth and maintenance cost before selecting pruning, age, count or mitigation options; distinguish display filtering from deleting stored gaps |

The categories below retain P2/P3 as relative backlog labels, not a chronological release schedule or references to the original audit phases.

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

Percent minimum penetration and ATR thresholds have been selected for an opt-in v1.2.0 product option; see [Break tolerance](BREAK_TOLERANCE.md). Cement completion options are the next design/comparison task. Other candidates below remain research; no experimental completion rule is included in the published source.

### Gate A — structural break acceptance

The baseline (None) accepts a strict boundary crossing using Close or High/Low. A small excess is still a break. The published source includes selectable Percent and ATR distances without changing that default; other acceptance alternatives remain research.

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

The immediate task is to define and illustrate the single-Cement baseline, two-Cement, consecutive-Cement and first-Cement-boundary alternatives. Pivot, retracement and ATR-correction variants remain in the wider research pool.

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

| Area | State / direction |
| --- | --- |
| Drawing reuse | Delivered v1.2.2 for working boundaries, classic Fibonacci and FVG display boxes |
| Drawing controls | Backlog: user-selected history retention and visual presets |
| FVG resources | Pending measurement: long-history growth, repeated splitting, searches/removals and storage cost |
| Reliability | Continue checking short-history startup, both break sources, Cement lookback 1–10 and tolerance modes |
| Settings documentation | Maintain with each change; current guides already exist |
| Confirmation policy | Preserve current realtime behavior; specify availability explicitly for any future mode |

## Alerts — deferred, last priority

No alert implementation is scheduled now. When resumed, start with **OTE contact**, then consider BOS/CHoCH, Cement and liquidity alerts.

OTE alert design must select first touch, close inside or re-entry; identify the working/reference swing; define behavior when OTE moves with a new extreme; and prevent repeated notifications while price remains in the same zone. A notification-at-close option must be distinguished from changing the structural engine's realtime evaluation policy.

## Updating this roadmap

Before starting a feature, select one item and turn it into a small scope with acceptance criteria. On completion, move its delivered behavior into the changelog and relevant guides. Keep unresolved alternatives in this roadmap rather than documenting them as implemented functionality.

See the [current algorithm](ALGORITHM.md), [parameters](PARAMETERS.md), [changelog](../CHANGELOG.md) and [contribution guide](../CONTRIBUTING.md) for the published behavior and change process.
