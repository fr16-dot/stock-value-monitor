---
name: fundamental-value-entry
description: Maintain a versioned, price-blind intrinsic-value registry and monitor public equities for RB/HV/EV entry zones without letting market price, momentum, options pricing, or analyst targets contaminate intrinsic value.
---

# Fundamental Value Entry

## Purpose

Maintain fixed, versioned intrinsic-value bands for a stock universe and monitor fresh market quotes against those bands.

Strict separation:

1. Valuation layer — fundamentals determine intrinsic value.
2. Execution layer — market price determines whether a locked valuation zone has been reached.
3. Action state — only user-confirmed trades change holdings.

Never reverse this direction.

## Core Policy

Active policy: **PRICE_BLIND_DUAL_TRACK_V3**.

When creating or reviewing intrinsic value, do not use current share price, recent performance, RSI, momentum, option IV, option premium, analyst price targets, or a desired entry price as valuation inputs.

Those data may be used only after valuation is locked, for execution/timing commentary.

Only material fundamental evidence may change valuation bands, including earnings/cash flow, guidance, margins, backlog, customer concentration, financing/debt/dilution/SBC/buybacks, M&A, regulatory decisions, major contracts/program milestones, or material competitive/technological change.

A price move by itself is never a reason to revise intrinsic value.

## Valuation Tracks

Choose the model class before calculating value.

### TRADITIONAL

Use where conventional operating economics are observable. Appropriate methods can include DCF, normalized EPS/FCF, EV/revenue, EV/EBITDA, or a triangulation.

### HIGH_CONVEXITY

Use for pre-profit, milestone-driven, binary, frontier-technology, or high-optionality businesses where a conventional steady-state multiple creates false precision.

Use milestone/scenario probability trees. Model technical, commercial, financing, regulatory, and execution milestones explicitly.

### Scenario Discipline

For either model class:

- Build Bear / Base / Bull cases.
- Probabilities must sum to 100%.
- Make scenario assumptions explicit.
- Model net cash/debt explicitly.
- Model expected future dilution explicitly when material.
- Avoid double-counting risk.

External fair values, analyst estimates, or third-party ratings are cross-checks only and must never anchor the model.

## Required Valuation Outputs

Each active valuation should support, where applicable:

- Bear range
- Base range
- Bull range
- Bear/Base/Bull probabilities
- Probability-weighted value (PW)
- RB zone
- HV zone
- EV zone
- confidence
- grade
- model_class
- scenario assumptions
- net cash per share
- expected dilution per share
- review notes
- review date

RB/HV/EV must be derived from the locked valuation framework, not inferred from current market price.

## Versioned Registry Contract

The **Versioned Valuation Registry** is the single source of truth.

1. Exactly one valuation version per ticker may be `ACTIVE`.
2. A materially changed valuation creates a new version and supersedes the prior version.
3. If a review finds no material change, keep the same version and update review metadata only.
4. Historical versions remain auditable.
5. Missing valuation is a valid state: `UNVALUED`.
6. An `UNVALUED` security may not be labeled RB, HV, EV, cheap, expensive, undervalued, or overvalued by this system until a formal price-blind valuation is built.

Recommended fields:

- identity: ticker, exchange, security/share class, currency
- versioning: version, status, review_date, supersedes
- model: model_class, route, grade, confidence
- scenarios: bear/base/bull ranges and probabilities, scenario_assumptions
- bridge: net_cash_per_share, expected_dilution_per_share
- zones: PW, RB, HV, EV
- cross-checks: external_fv, external_source, external_as_of, external_rating_notes
- notes: review_notes

Legacy route codes `V` / `H` may remain in old exports; `model_class` is the clearer V3 field.

## Security Identity Gate

Before applying a valuation to a quote, validate that the quote belongs to the same security represented by the registry record.

Verify the relevant combination of ticker, exchange/venue, security/share class, currency, and corporate-action continuity.

If identity is ambiguous, block zone classification rather than guessing.

## Quote Freshness Gate

Every execution-layer scan must validate quote timestamp and trading session.

A stale, missing, or session-ambiguous quote must not trigger a price-zone alert.

Record where available:

- current_price
- snapshot_time
- session
- day_low
- day_high
- previous_close
- last_seen_price

## Monitoring Logic

For each security:

1. Validate security identity.
2. Load the single ACTIVE valuation.
3. If no valid ACTIVE valuation exists, mark `UNVALUED` and do not classify a price zone.
4. Obtain a fresh quote and pass the quote-freshness gate.
5. Classify price against locked RB/HV/EV zones.
6. Evaluate alert triggers.
7. Update MarketState only after successful validation.

### Alert Triggers

Trigger on:

- first entry into RB, HV, or EV
- transition into a deeper value zone
- crossing a stored `next_add` threshold
- a meaningful additional decline while remaining in the same zone, provided fundamentals have not materially deteriorated
- a material catalyst requiring immediate valuation review

Default same-zone re-alert threshold:

- general universe: **7%**
- focus names: **5%**

Current focus set from migrated configuration:

`VRT, CRDO, AVGO, HPS.A, ASML, NVDA`

If the intraday low entered a deeper zone and price later rebounded, report the zone actually touched and note the rebound.

## Catalyst-Forced Review

A material catalyst must force a fundamentals review before relying on the old valuation for a fresh recommendation.

Examples: earnings, guidance, financing/dilution, M&A, regulatory decisions, major contract awards/losses, backlog changes, major customer changes, or meaningful balance-sheet events.

Valid outcomes:

- No material valuation change → retain current version; refresh review metadata.
- Material valuation change → create a new ACTIVE version and supersede the old one.

Never silently move RB/HV/EV bands to follow price.

## MarketState and AlertState

Useful runtime fields:

- current_price
- snapshot_time
- session
- day_low/day_high
- previous_close
- current_zone/previous_zone
- last_seen_price
- last_alert_price
- last_alert_zone
- last_alert_time
- next_trigger
- next_add

Keep runtime market state separate from versioned intrinsic-value records.

## ActionState and Holdings

A recommendation is not a trade.

`user_action` should distinguish at least:

- `UNKNOWN`
- `CONFIRMED_BOUGHT`
- `CONFIRMED_SOLD`
- `REJECTED`

Only explicit user confirmation, broker evidence, or equivalent evidence may update holdings.

## Report Format

For monitoring and daily archives, place value opportunities in this order:

1. **极高价值（EV）**
2. **高价值（HV）**

If a section is empty, explicitly show **无**.

Keep RB and non-value-zone names below EV/HV unless the user requests another layout.

## Close Archive

The close archive is intended to run **10 minutes after the U.S. regular market close**, normally **16:10 America/New_York on trading days**, using regular-session close data.

Named clock schedules should use exact scheduling rather than a flexible window.

## Options / LEAP Boundary

LEAP Watch and option pricing belong to the execution layer.

Option premiums, IV, skew, or market-implied probabilities may help evaluate implementation, convexity, or timing, but they must not be used to back-solve or modify intrinsic value.

## Audit Rules

Periodically audit the universe for:

- missing ACTIVE valuations
- multiple ACTIVE versions
- missing RB/HV/EV fields
- stale review metadata
- invalid security identity
- stale quote state
- inconsistent action/holdings state

A newly added ticker with no formal valuation must be queued for valuation before the monitor may classify it.

## Failure-Safe Behavior

When a prerequisite fails:

- no valuation → `UNVALUED`, no zone judgment
- ambiguous identity → no classification
- stale/invalid quote → no price alert
- material catalyst not yet reviewed → mark review required
- unclear trade confirmation → do not change holdings

It is better to withhold a zone label than to produce a false one.
