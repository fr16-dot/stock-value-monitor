# stock-value-monitor

A price-blind, versioned equity valuation and monitoring system.

## Architecture

The system separates three concerns:

1. **Valuation kernel** — builds and versions intrinsic value without current-price anchoring.
2. **Monitoring scheduler** — performs inexpensive FOCUS/CORE/EXTENDED scans.
3. **Event escalation** — queues deep research on material catalysts or abnormal moves and reconciles it in one scheduled post-close heavy review per U.S. trading date.

## Canonical skill package

`skill/fundamental-value-entry/`

- `SKILL.md` — compact runtime contract and invariants
- `references/valuation-models.md` — TRADITIONAL and HIGH_CONVEXITY models
- `references/monitoring-policy.md` — FOCUS / CORE / EXTENDED behavior
- `references/review-triggers.md` — 8%/5% abnormal-move rules and post-close idempotency
- `references/registry-schema.md` — valuation/runtime state schema
- `references/source-policy.md` — source hierarchy and freshness
- `references/momentum-entry-framework.md` — optional execution overlay
- `references/lessen_learnrd.md` — incident-derived permanent controls
- `agents/openai.yaml` — skill UI metadata
- `assets/icon.svg` — skill icon

## Repository config

- `config/defaults.yaml` — compatibility defaults
- `config/universe.yaml` — tier definitions and focus order
- `config/monitoring.yaml` — lightweight scan and heavy-review policy
- `config/execution-policy.yaml` — separation of valuation, momentum, and holdings state

## Key controls

- FOCUS and CORE are `daily_core: true`; EXTENDED is `daily_core: false`.
- A default absolute single-session move of 8% queues deep review; FOCUS uses 5%.
- The price trigger causes investigation, not automatic revaluation.
- Scheduled heavy review runs exactly once per U.S. trading date, normally at 16:10 America/New_York.
- HIGH_CONVEXITY is a first-class valuation model for milestone-driven and financing-sensitive businesses.
- Hard-coded tranche percentages are removed from the valuation kernel.
