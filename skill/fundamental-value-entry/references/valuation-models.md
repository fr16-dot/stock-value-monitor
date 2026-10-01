# Valuation Models

## Core rule

Select `model_class` before selecting valuation methods. Current market price is not an input to intrinsic-value construction.

## TRADITIONAL

Use when revenue, margins, cash conversion, and normalized economics can be forecast with reasonable discipline.

### Suitable methods

- DCF / normalized FCF
- normalized forward P/E
- EV/EBITDA
- EV/FCF
- EV/revenue plus explicit steady-state margin assumptions
- asset value or segment SOTP when appropriate

Use at least two independent methods when practical. Reconcile differences rather than averaging mechanically.

### Required scenario fields

- bear/base/bull operating assumptions
- bear/base/bull valuation assumptions
- scenario probabilities summing to 100%
- net cash/debt bridge
- expected share-count change
- probability-weighted value

## HIGH_CONVEXITY

Use when value depends heavily on future milestones rather than current normalized earnings.

Examples include pre-profit frontier technology, quantum, space, emerging defense/autonomy, biotech-like milestone economics, new infrastructure platforms, and other financing-sensitive optionality businesses.

### Required probability tree

Separate major branches such as:

1. technical feasibility / product performance;
2. regulatory or certification outcome;
3. production or deployment scale;
4. commercial adoption / contracted demand;
5. unit economics and margin realization;
6. financing runway and dilution;
7. strategic optionality that is independently supportable.

Do not double-count the same risk by both sharply reducing the success probability and applying an unjustifiably depressed success-case valuation.

### Cash runway and dilution

Model current cash, expected burn, financing timing, and expected dilution explicitly. A strong narrative with insufficient financing runway is not equivalent to a funded success case.

### Optionality discipline

Do not sum the full value of mutually dependent or mutually exclusive future businesses. Apply explicit probabilities and dependencies.

## Scenario discipline

For all corporate model classes:

- Bear/Base/Bull probabilities sum to 100%.
- Assumptions are written before current price is used for execution.
- Analyst targets and third-party fair values are cross-checks only.
- Reverse DCF may describe market expectations but does not define intrinsic value.
- Valuation uncertainty should widen ranges and lower confidence rather than create fake precision.

## Fixed entry zones

RB/HV/EV are derived from intrinsic value, downside, quality, uncertainty, and required return after the valuation model is locked.

They are versioned outputs. A market-price move cannot by itself move the zones.
