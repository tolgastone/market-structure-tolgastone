# The MST approach

Cement is the foundation of MST: a precise close-based candle rule used to establish initial direction and confirm impulse completion. MST describes market structure through reference boundaries, directional impulses, and completion. In the established structural cycle, a boundary break starts an impulse. The engine follows its extreme until opposite-direction Cement confirms completion, unless an opposite boundary break interrupts it first.

The drawings describe this sequence. They are not a conventional fixed-window pivot detector: the source does not use `ta.pivothigh` or `ta.pivotlow`.

For candle diagrams and a plain-language introduction, see [Illustrated concepts](CONCEPTS.md).

## Reference swing and working swing

The engine distinguishes two swings:

- **Reference swing (`lastSwing`):** the stored swing whose high and low are used for break tests.
- **Working swing (`currentSwing`):** the swing being tracked or carried between stages. Its active extreme can extend during an impulse.

Separate candidate high/low points help anchor the next swing. The connecting leg drawn on a continuation links the preceding extreme to the tracked opposite candidate.

The high and low form structural boundaries. The script draws extreme points as circles; it does not assign literal HH/HL/LH/LL text labels.

## Boundary breaks and BOS/CHoCH

| Setting | Upward break | Downward break |
| --- | --- | --- |
| Close (default) | Current close > reference high | Current close < reference low |
| High/Low | Current high > reference high | Current low < reference low |

An equal touch is not a break. Close selects a price source; it does not defer execution until bar close.

For the normal break path, classification compares break direction with reference direction:

| Reference direction | Upward break | Downward break |
| --- | --- | --- |
| UP | BOS | CHoCH |
| DOWN | CHoCH | BOS |

The dashed break line ends at the break bar. Its label is centered between the reference endpoint and the break bar, so the label's horizontal position is not the event time.

## Cement confirms impulse completion

For length N, a bullish Cement closes strictly above **every high of the preceding N bars**. A bearish Cement closes strictly below **every low of the preceding N bars**. The default N is 1.

N is not a count of consecutive Cement candles. Candle body color is not part of the predicate.

During an UP impulse, the engine extends the high and normally completes the impulse on bearish Cement. During a DOWN impulse, it extends the low and normally completes on bullish Cement. The extreme update occurs before the Cement completion check, so a completion bar can also supply the final extreme.

Cement completes the normal impulse; it is not the normal structural-break trigger. Hiding Cement markers does not disable Cement calculations.

## A normal continuation example

Consider an established UP reference with a high of 100, and a tracked candidate low at 95. Assume the engine is waiting for a break and no opposite interruption occurs. Prices below are illustrative.

| Stage | Example | Engine behavior |
| --- | --- | --- |
| Break | Close reaches 101, above the reference high of 100 | Draw an upward BOS and start the UP impulse from the candidate low |
| Extend | A later bar reaches high 110, low 107, close 109 | Extend the working high to 110 |
| Complete | The next bar has high 109, low 104, close 106 | With N=1, 106 < 107 is bearish Cement; complete the UP swing at high 110 |

The swing endpoint belongs to the bar that made 110. The completed swing drawing becomes available on the later completion bar. A closed-bar historical drawing does not establish what appeared during each earlier live update.

The downward case mirrors this: break a reference low, track lower extremes, and normally complete on bullish Cement.

## Initialization and interruption

**Initial direction.** The first loaded bar seeds both extremes. Before a mature reference exists, the first Cement establishes initial direction and seeds a provisional opposite reference. This bootstrap is different from the established boundary-break cycle. The history loaded into the chart therefore matters.

**Opposite-boundary interruption.** While tracking an impulse, a break through the opposite reference boundary takes priority over the normal extreme-update/Cement path. The engine draws and copies the interrupted working swing, creates the reverse working swing, and evaluates break drawing against the newly copied reference. It can finalize an impulse without opposite Cement.

**Same-bar sequencing.** The three state blocks execute as sequential independent checks. A state change can enter the next block on the same bar. In High/Low mode, an outside bar can satisfy both normal break tests; the downward test follows the upward test in source order. These details are part of the current implementation.

## Fibonacci and visible boundaries

Displayed Fibonacci levels use the working swing and logarithmic prices. For high H, low L and ratio r:

- UP: `exp(log(H) - (log(H) - log(L)) * r)`
- DOWN: `exp(log(L) + (log(H) - log(L)) * r)`

At r=0.5, the price is the geometric midpoint `sqrt(H * L)`. Levels 0 and 1 are always included when Fibonacci is shown. Positive prices are required by these calculations.

The current boundary arrows also follow the working swing. Actual break predicates use the reference swing. The arrows should not be assumed to show the exact reference levels throughout an extending impulse.

## Fair value gaps

The FVG detector is independent of Cement and structural break classification:

- Bearish gap: current high < low two bars earlier.
- Bullish gap: current low > high two bars earlier.

No middle-candle displacement requirement is applied. Each current candle range is subtracted from stored gaps: full overlap removes a gap, partial overlap trims it, and a strictly interior range splits it into surviving fragments. This is not a first-touch-filled rule. Some equality-touch cases do not trim a fragment.

Storage is maintained even when FVG display is hidden. Display requires the entire fragment to lie within a log-expanded reference-swing range: upper bound inclusive, lower bound strict. Boxes extend from the current bar to 30 bars ahead, rather than from the original gap bar.

## Availability and current limits

Open-bar breaks, Cement predicates, FVGs and drawings can change before close. No close-confirmation guard is applied. Three different times must be distinguished: the extreme bar, the completion/break bar, and the moment an observation becomes final at close.

The script allows up to 500 lines, 500 labels and 500 boxes. Older drawings may disappear as budgets are reached. FVG storage has no explicit age or count cap. Very short loaded histories may not establish a reference swing. Long histories and repeated gap splitting can increase resource use; the script does not cap stored FVG age or count.
