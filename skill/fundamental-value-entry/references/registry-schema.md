# Registry schema

The active registry is versioned and price-blind.

## Core valuation fields

| Group | Fields |
|---|---|
| Identity | ticker, exchange, security_class, currency |
| Versioning | version, status, review_date, supersedes |
| Model | model_class, route, grade, confidence |
| Bear | bear_low, bear_high, bear_probability |
| Base | base_low, base_high, base_probability |
| Bull | bull_low, bull_high, bull_probability |
| Probability-weighted | pw_low, pw_high |
| Zones | rb_low, rb_high, hv_low, hv_high, ev_operator, ev_value |
| Bridge | net_cash_per_share, expected_dilution_per_share |
| Evidence | scenario_assumptions, review_notes |
| External cross-check | external_fv, external_source, external_as_of, external_rating_notes |

## Invariants

- One ACTIVE valuation version per ticker.
- Material valuation change => new version.
- No material valuation change => same version, refreshed review metadata.
- No ACTIVE valuation => UNVALUED.
- Market price never changes intrinsic value by itself.
