# Lessons Learned

The filename intentionally preserves the historical spelling used by the project.

## Never classify an unvalued ticker

Missing valuation is `UNVALUED`, not an invitation to infer cheapness from price.

## Price must not contaminate intrinsic value

Sharp moves can bias fair-value judgment. Price-only changes never revise valuation bands.

## Do not double-count convexity risk

For HIGH_CONVEXITY names, do not punish the same risk both through probability and through an unjustifiably depressed success-case value.

## Catalysts require review before reconfirmation

A material event can stale the previous thesis. Preserve the old version until review concludes; do not silently treat it as reconfirmed.

## Quote freshness is a gate

A stale or wrong-session quote cannot trigger or suppress an alert.

## Identity must be verified

Ticker reuse, share-class changes, ADR changes, mergers, rebrands, and listing transfers can invalidate inherited facts.

## Recommendation is not execution

Suggested trades do not become holdings unless the user confirms execution or equivalent evidence exists.

## Avoid repeated expensive research

Use lightweight scans during the day. Queue deep work and consolidate it into the single scheduled post-close heavy review for each U.S. trading date.

## Large moves need diagnosis, not automatic revaluation

The default 8% abnormal-move rule, with a 5% FOCUS override, creates a review obligation. It does not change intrinsic value by itself.

## Preserve intraday zone touches

If the day low entered a deeper locked value zone and later rebounded, preserve the touch in monitoring state and reporting.
