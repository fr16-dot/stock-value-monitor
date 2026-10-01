# lessen_learnrd.md

This file records failures and corrections from the stock-value monitoring workflow. The filename intentionally preserves the original requested spelling.

## 1. Never classify an unvalued ticker

New names must not be discussed as RB/HV/EV until a formal fixed valuation exists.

**Permanent rule:** missing valuation is `UNVALUED`; run registry-completeness checks whenever new tickers are added.

## 2. Price must not contaminate intrinsic value

Sharp price moves can psychologically pull fair value up or down.

**Permanent rule:** price/momentum stays out of intrinsic-value construction. Only material fundamental evidence changes valuation bands.

## 3. Do not double-count risk

For HIGH_CONVEXITY names, do not penalize the same risk once through probability and again through an unnecessarily depressed success-case valuation.

**Permanent rule:** separate probability risk from economic outcome assumptions.

## 4. Catalysts require forced review

Earnings, guidance, financing, dilution, M&A, regulatory events, major contracts, backlog changes, customer changes, and balance-sheet events can stale a fixed valuation.

**Permanent rule:** a catalyst review either keeps the same version with refreshed metadata or creates a new superseding version.

## 5. Validate quote freshness

A technically valid price can still be operationally wrong if stale or from the wrong session.

**Permanent rule:** stale or session-ambiguous quotes cannot generate alerts.

## 6. Validate security identity

Ticker reuse, exchanges, share classes, currencies, and corporate actions can mismatch quote and valuation.

**Permanent rule:** ambiguous identity blocks classification.

## 7. Recommendation is not execution

Suggested position sizes or “buy” language must not leak into holdings state.

**Permanent rule:** only explicit confirmation or equivalent evidence changes holdings.

## 8. Same-zone suppression can hide important moves

A stock can fall materially deeper inside the same HV/EV zone without crossing a zone boundary.

**Permanent rule:** re-alert after an additional decline of 7% for the general universe or 5% for focus names, assuming fundamentals remain intact.

## 9. Intraday touch matters after rebound

Do not erase an HV/EV touch because the latest price rebounded above the threshold.

**Permanent rule:** preserve day-low zone touches in MarketState and reports.

## 10. Scheduled close reports should be exact

A close archive tied to a named market-close offset should not use flexible scheduling.

**Permanent rule:** run the close archive at 16:10 America/New_York on trading days, subject to exchange holidays and early closes.
