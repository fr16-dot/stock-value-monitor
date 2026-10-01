# Review Triggers

## Review reasons

Use one or more explicit `review_reason` values:

- `EARNINGS`
- `GUIDANCE`
- `PRICE_ANOMALY`
- `FINANCING`
- `DILUTION`
- `M_AND_A`
- `REGULATORY`
- `CONTRACT`
- `CUSTOMER`
- `BACKLOG`
- `TECHNOLOGY`
- `COMPETITION`
- `IDENTITY_CHANGE`
- `VALUATION_STALE`
- `MANUAL`
- `SCHEDULED_CLOSE`

## Abnormal-move rule

Default rule:

`abs(single_session_return) >= 8%` → set `deep_review_pending: true`, reason `PRICE_ANOMALY`.

FOCUS override:

`abs(single_session_return) >= 5%` → set `deep_review_pending: true`, reason `PRICE_ANOMALY`.

The trigger requires explanation, not automatic revaluation. Determine whether the move reflects company-specific fundamentals, macro/sector beta, positioning, liquidity, rumor, or bad data.

## Material catalyst rule

A material earnings, guidance, financing, dilution, M&A, regulatory, contract/order, backlog, customer, licensing, production, or competitive event queues deep review regardless of tier.

This allows EXTENDED names to bypass lightweight-only treatment.

## Scheduled heavy-review rule

Run exactly one scheduled heavy-review job for each U.S. trading date, normally at 16:10 America/New_York.

Use `heavy_review_trading_date` as the idempotency key.

Before starting:

1. Determine the applicable U.S. trading date.
2. Check whether the scheduled heavy review is already `COMPLETED` for that date.
3. If yes, do not run a duplicate scheduled heavy review.
4. If no, mark the job `RUNNING`, process the queue, persist results, then mark `COMPLETED`.

If interrupted, retain enough state to distinguish `RUNNING/FAILED` from `COMPLETED`; never treat an incomplete run as completed.

## Heavy-review scope

The consolidated post-close job:

- reconciles the day's material filings, earnings, guidance, and events for FOCUS/CORE;
- processes all `deep_review_pending` securities, including EXTENDED;
- performs full model reconstruction only when the evidence requires it;
- otherwise confirms no material valuation change and refreshes review metadata;
- records the reason, evidence, result, and valuation-version decision.

## Manual review

A user-requested deep dive may run at any time. It is separate from the scheduled idempotency key and does not authorize a second scheduled heavy-review job for the same trading date.
