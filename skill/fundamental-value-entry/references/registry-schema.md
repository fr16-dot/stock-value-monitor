# Registry Schema

## ValuationVersion

Required fields for corporate securities:

- `ticker`
- `issuer_name`
- `exchange`
- `currency`
- `security_class`
- `version`
- `status` (`ACTIVE` or historical)
- `review_date`
- `model_class` (`TRADITIONAL` or `HIGH_CONVEXITY`)
- `business_grade`
- `confidence`
- `bear_low`, `bear_high`, `bear_probability`
- `base_low`, `base_high`, `base_probability`
- `bull_low`, `bull_high`, `bull_probability`
- `pw_low`, `pw_high`
- `rb_low`, `rb_high`
- `hv_low`, `hv_high`
- `ev_operator`, `ev_value`
- `net_cash_per_share`
- `expected_dilution_per_share`
- `scenario_assumptions`
- `valuation_change_reason`
- `review_notes`
- `supersedes`

Invariant: exactly one ACTIVE valuation per valued corporate security.

## UniverseRecord

- `ticker`
- `tier` (`FOCUS`, `CORE`, `EXTENDED`)
- `daily_core`
- `active`
- `valuation_status`
- `monitoring_mode`

Invariant: `FOCUS` and `CORE` imply `daily_core: true`; `EXTENDED` implies `daily_core: false` unless explicitly overridden and documented.

## MarketState

- `ticker`
- `current_price`
- `snapshot_time`
- `session`
- `previous_close`
- `day_low`
- `day_high`
- `single_session_return`
- `current_zone`
- `previous_zone`
- `last_seen_price`

## AlertState

- `last_alert_zone`
- `last_alert_price`
- `last_alert_time`
- `next_trigger`
- `next_add`

## ReviewState

- `deep_review_pending`
- `review_reason`
- `pending_since`
- `last_light_scan_at`
- `last_heavy_review_at`
- `heavy_review_trading_date`
- `heavy_review_status` (`NOT_STARTED`, `RUNNING`, `COMPLETED`, `FAILED`)

## ActionState

- `recommendation_status`
- `user_action` (`UNKNOWN`, `CONFIRMED_BOUGHT`, `CONFIRMED_SOLD`, `REJECTED`)
- `confirmed_at`

Recommendation state and confirmed holdings state must never be conflated.
