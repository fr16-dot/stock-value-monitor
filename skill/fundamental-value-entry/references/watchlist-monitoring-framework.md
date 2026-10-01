# Watchlist Opportunity Monitoring

Read this reference for scheduled monitoring, portfolio-wide selloffs, “what is buyable now?” scans, and missed-opportunity reviews.

## 1. Preserve three independent states

For every ticker, retain:

1. **Valuation record:** fixed zones, assumptions version, confidence, and fundamental invalidation conditions.
2. **Market state:** current price/time/session, observed zone, event levels, and material news.
3. **Action state:** last alert, next-add trigger, recommended tranche, and user-confirmed execution.

“Recommended” is not “executed.” Use holding/add language only after the user confirms ownership or supplies a current portfolio snapshot.

## 2. Run order

1. Load the latest dated valuation record without looking at the new price.
2. Check filings, IR releases, guidance, financing, dilution, competition, and capital structure.
3. If the record is absent or stale after a material event, build a price-blind provisional bear/base/bull record in the same run.
4. Retrieve the latest time-stamped price and session type.
5. Map price to the fixed zone and predefined next-add levels.
6. Test single-name, same-zone, cluster, momentum, and thesis-change triggers.
7. Rank eligible actions by risk-adjusted payoff and portfolio fit.
8. Save the observed state even when no notification is sent.

Do not silently skip a ticker because complete consensus data is unavailable. Lower confidence, widen scenarios, and cap size.

## 3. Notification triggers

Notify on any first occurrence:

- The stock enters reasonable-buy, high-value, or extremely-attractive territory.
- It moves into a deeper zone.
- It remains in the same broad zone but crosses the recorded next-add price, the zone midpoint, or falls at least 7% below the last alerted price with the thesis intact.
- A fundamental input or valuation version materially changes.
- A verified catalyst becomes chase-qualified or a prior momentum event fails.
- A portfolio-wide opportunity cluster forms.
- A prior recommendation has unknown execution status and a new, deeper action level is reached.

Do not notify merely because the price oscillates around a boundary. Use the formal close to confirm a zone; label premarket and intraday alerts provisional.

## 4. Action floors

Use the grade/zone action-floor matrix in SKILL.md. The percentage is of the final target position, not the whole portfolio.

Rules:

- Reduce the initial A/B tranche by roughly half for earnings within two sessions, unusually wide intraday spreads, or a portfolio correlation constraint.
- Never reduce an intact A/B buy-zone signal to 0% solely because the stock may fall further or technical confirmation is absent.
- For C/D names, require adequate runway, quantified dilution, and a defined failure point; otherwise alert the valuation change but recommend 0% and explain the exception.
- Provide a no-pullback completion path so confirmation does not create a second reason to wait.

## 5. Portfolio-wide cluster alert

Create a cluster when:

- Three or more A/B companies are simultaneously reasonable-buy or better; or
- Five or more monitored stocks fall at least 5% in one session, with a common macro/sector cause and mostly intact theses.

Cluster handling:

1. Rescan the entire list, including names already alerted in their current zone.
2. Rank by `(quality × valuation confidence × base upside) / bear downside`, then penalize leverage, dilution, and correlated exposure.
3. Select at most five actions.
4. Recommend an aggregate first deployment, normally 5%–12% of portfolio value, bounded by available cash and existing concentration.
5. Allocate more to A/B quality and less to high-capex or reflexive names.
6. Reserve follow-on cash, but do not let the reserve make the first deployment trivial.

A cluster alert is a separate state from individual zone alerts and is not suppressed by prior per-ticker notifications.

## 6. Deduplication and escalation

Track `last_observed_zone` and `last_alerted_zone` separately.

- Same price/zone/thesis/next-add state: remain silent.
- Deeper zone or next-add trigger: alert once.
- New cluster: alert once even if components were individually alerted.
- Premarket/intraday provisional alert: send a short formal-close confirmation only if the signal remains valid or changes.
- If a recommended trade was not user-confirmed, keep execution status unknown; do not change it to held.
- If the user declines a trade, do not repeat the same trigger; re-alert only on a deeper level, thesis change, or new qualified catalyst.

## 7. Required monitoring output

Lead with one sentence stating whether this is an individual opportunity, a portfolio-wide deployment window, a thesis change, or no actionable change.

For each of at most five names include:

- Price, timestamp, and session label.
- Fixed zone, base value/upside, and bear downside.
- Grade, valuation confidence, and thesis-change status.
- Value signal and momentum signal.
- Exact target weight and tranche to execute now.
- Next-add price and no-pullback path.
- Separate value and momentum invalidation conditions.
- Holding language only when user-confirmed.

For a cluster, add aggregate deployment amount/percentage, sector-correlation cap, cash retained, and the strongest excluded candidates with reasons.

## 8. Missed-opportunity audit

When reviewing a miss, judge the contemporaneous process rather than subsequent returns:

- Was a valid fixed zone available at the time?
- Was the latest fundamental information already public?
- Did uncertainty justify smaller size or genuinely justify zero?
- Was a cluster present but never synthesized?
- Did deduplication, stale records, or assumed execution suppress the signal?
- Did the plan include an executable first tranche and no-pullback path?

Convert every identified failure into a durable rule or state field. Do not move historical intrinsic values merely to make the post-hoc result look predictable.
