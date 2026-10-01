---
name: fundamental-value-entry
description: Maintain price-blind, versioned intrinsic-value records; monitor a tiered stock universe; escalate material events and abnormal price moves into controlled deep review; and keep valuation, momentum, and execution state separate. Use for fair value, RB/HV/EV entry zones, watchlist monitoring, post-earnings review, abnormal-move diagnosis, catalyst review, and valuation-registry maintenance. Do not use current market price, momentum, options pricing, or analyst targets to back-solve intrinsic value.
---

# Fundamental Value Entry

## Purpose

Operate three separate layers:

1. **Valuation kernel** — estimate intrinsic value without current-price anchoring.
2. **Monitoring scheduler** — run cheap, repeatable checks across the watchlist.
3. **Event escalation** — queue expensive deep review only when evidence justifies it.

The execution layer may use market price after valuation is locked. It may not rewrite intrinsic value merely because price moved.

## Read supporting policies

Use these references when relevant:

- `references/valuation-models.md` — model-class selection and valuation construction.
- `references/monitoring-policy.md` — FOCUS / CORE / EXTENDED monitoring behavior.
- `references/review-triggers.md` — deep-review triggers and post-close review rules.
- `references/registry-schema.md` — versioned valuation and runtime-state fields.
- `references/source-policy.md` — source hierarchy and freshness requirements.
- `references/momentum-entry-framework.md` — optional momentum/tape execution overlay.
- `references/lessen_learnrd.md` — incident-derived permanent controls.

## Non-negotiable invariants

1. **Price blind first.** Build or review intrinsic value before using current market price for execution.
2. **Classify before valuing.** Assign `model_class` before selecting valuation methods.
3. **One ACTIVE valuation.** Each corporate security has exactly one ACTIVE valuation version or is explicitly `UNVALUED`.
4. **UNVALUED means no zone.** Never label an unvalued security RB, HV, EV, cheap, expensive, undervalued, or overvalued.
5. **Price does not revise value.** Price, RSI, momentum, IV, option premium, analyst target, and recent return cannot by themselves change intrinsic value.
6. **Material evidence only.** New results, guidance, capital structure, dilution, competition, contracts, regulation, M&A, or other material fundamental evidence may justify a new valuation version.
7. **Identity gate.** Verify ticker, issuer, exchange, currency, security/share class, and corporate-action continuity before mapping a valuation to a quote.
8. **Freshness gate.** A stale, delayed, wrong-session, or wrong-exchange quote cannot drive zone mapping or a no-action conclusion.
9. **Recommendation is not execution.** Suggested trades never update holdings without explicit user confirmation or equivalent evidence.
10. **Runtime state is not valuation state.** MarketState, AlertState, ReviewState, and ActionState remain separate from ValuationVersion.
11. **Scheduled heavy review is idempotent.** Run the scheduled post-close heavy review exactly once per U.S. trading day.
12. **Abnormal movement triggers review, not revaluation.** A large move queues investigation; valuation changes only if the investigation finds material fundamental evidence.

## Universe tiers

Monitoring priority is independent of valuation quality.

### FOCUS

- `daily_core: true`
- Highest monitoring priority.
- Included in every scheduled lightweight scan.
- Default abnormal-move threshold: `5%` absolute single-session move.
- Use for names most likely to require near-term action.

### CORE

- `daily_core: true`
- Standard core watchlist.
- Included in the standard daily monitoring cycle.
- Default abnormal-move threshold: `8%` absolute single-session move.
- Use for actively followed, formally valued securities.

### EXTENDED

- `daily_core: false`
- Low-cost radar universe.
- No routine deep research solely because the ticker is present.
- Escalate on a material catalyst, abnormal move, identity event, stale valuation requiring refresh, or explicit user request.
- Default abnormal-move threshold: `8%` when a valid quote is observed.

Tier changes affect monitoring frequency only. They do not change fair value, model class, business grade, or valuation zones.

## Model classes

Every corporate valuation must declare one of these model classes before calculation.

### TRADITIONAL

Use when operating economics and normalized cash generation are observable enough for conventional forecasting.

Typical methods: DCF, normalized EPS, EV/EBITDA, EV/FCF, EV/revenue with steady-state margins, or a triangulation of suitable methods.

### HIGH_CONVEXITY

Use for pre-profit, milestone-driven, binary, frontier-technology, platform-optionality, or financing-sensitive businesses where conventional steady-state multiples create false precision.

Build a probability tree around technical, commercial, regulatory, financing, production, and adoption milestones. Model cash runway and expected dilution explicitly. Do not assign full value to every optional future business simultaneously.

### Non-corporate modes

ETFs and indices may use explicit non-corporate modes such as `NAV_UNDERLYING` or `INDEX_ONLY`. Do not force corporate intrinsic-value logic onto them.

See `references/valuation-models.md` for full construction rules.

## Valuation workflow

1. Resolve security identity.
2. Assign universe tier and `model_class`.
3. Gather current primary-source fundamentals.
4. Grade business quality and identify thesis drivers.
5. Lock price-blind Bear / Base / Bull assumptions.
6. Apply suitable valuation methods for the model class.
7. Model net cash/debt, dilution, and per-share bridge explicitly.
8. Derive probability-weighted value and fixed RB / HV / EV zones.
9. Save or review the ACTIVE valuation version.
10. Only then retrieve current price and map it to the locked zones.

External fair values, consensus estimates, analyst targets, and market-implied multiples are cross-checks only. They must not anchor the model.

## Monitoring workflow

Lightweight scans are intentionally cheap. They do not rebuild the valuation model.

For each eligible ticker:

1. Pass the identity gate.
2. Load the single ACTIVE valuation or explicit non-corporate model.
3. Pass the quote-freshness gate.
4. Record price, timestamp, session, previous close, day high/low, and day change.
5. Map the price to locked RB/HV/EV zones only if valuation and quote are valid.
6. Check material-catalyst flags and abnormal-move thresholds.
7. If a deep-review trigger fires, set `deep_review_pending: true` with a `review_reason`.
8. Update MarketState and AlertState; do not silently change ValuationVersion.

## Deep-review triggers

Queue deep review when any of the following occurs:

- earnings, guidance, or a material investor update;
- financing, dilution, debt, buyback, or major capital-allocation change;
- M&A or a material asset transaction;
- regulatory approval, rejection, investigation, or material rule change;
- major contract, order, backlog, customer, licensing, or production event;
- material competitive or technological change;
- security identity or corporate-action change;
- stale valuation that breaches the configured freshness policy;
- absolute single-session move of at least `8%` by default;
- FOCUS override: absolute single-session move of at least `5%`;
- explicit user request.

A price trigger means **investigate why**. It does not mean **change fair value**.

## Scheduled post-close heavy review

Run one consolidated heavy-review job after each U.S. trading day, normally at `16:10 America/New_York`.

The scheduled job must be idempotent by U.S. trading date:

- if that trading date already has a completed scheduled heavy review, do not run a second scheduled heavy review;
- process all `deep_review_pending` names;
- reconcile the day's filings, earnings, guidance, material news, and abnormal moves for FOCUS/CORE;
- include pending EXTENDED names;
- clear a pending flag only after the review result is persisted;
- preserve the existing valuation version if fundamentals are not materially changed;
- create a new ACTIVE version only when material evidence changes the valuation assumptions.

An explicit user-requested manual deep dive is separate from the scheduled once-per-day job and does not permit duplicate scheduled runs.

See `references/review-triggers.md` for review reasons and idempotency rules.

## Versioning rules

Each valuation record must carry a version identifier, status, review date, model class, assumptions, and change reason.

- Material valuation change → create a new version, mark it ACTIVE, supersede the prior version.
- No material valuation change → keep the same version and refresh review metadata only.
- No valid valuation → `UNVALUED`; do not infer zones.
- Never silently move RB/HV/EV bands to follow price.

## Momentum and execution boundary

Momentum is an optional execution overlay, not part of intrinsic-value construction.

After valuation is locked, the momentum module may use catalyst quality, event levels, relative volume, gap behavior, and market/sector-relative strength to decide timing. It may not change fair value.

Position sizing, tranche percentages, target weights, and portfolio concentration rules belong to execution policy or user context, not the valuation kernel. Do not hard-code them into intrinsic-value rules.

## Required monitoring outputs

For watchlist scans, report:

- EV names first;
- HV names second;
- RB / other names only when useful;
- `UNVALUED`, stale-quote, identity-blocked, or review-pending names explicitly;
- any newly fired deep-review trigger and its reason;
- whether today's scheduled heavy review has completed.

If EV or HV is empty, state `无` rather than hiding the section.

## Audit checks

Regularly test for:

- zero or multiple ACTIVE valuation versions;
- missing `model_class`;
- missing RB/HV/EV fields on valued corporate securities;
- stale review metadata;
- stale or ambiguous quotes;
- identity mismatch;
- unresolved `deep_review_pending` state;
- duplicate scheduled heavy review for the same U.S. trading date;
- recommended trades incorrectly marked as executed.

When an invariant fails, block the affected classification rather than guessing.
