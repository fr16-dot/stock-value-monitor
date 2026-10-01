# Monitoring Policy

## Objective

Keep frequent monitoring cheap and deterministic. Expensive fundamental research is event-driven and consolidated after the close.

## Tier semantics

| Tier | daily_core | Priority | Lightweight monitoring | Default abnormal-move trigger |
|---|---|---|---|---:|
| FOCUS | true | highest | every scheduled lightweight scan | 5% |
| CORE | true | standard | standard daily monitoring cycle | 8% |
| EXTENDED | false | low | event/anomaly driven; no routine deep research | 8% when observed |

Tier affects monitoring frequency only. It never changes intrinsic value or business quality.

## Lightweight scan

A lightweight scan may retrieve:

- valid quote and timestamp;
- previous close and session change;
- day high/low;
- locked valuation zone;
- filing/earnings/material-event flag;
- identity status;
- pending-review status.

It must not rebuild a full model merely because the scan ran.

## Alert logic

Alert on:

- first entry into RB/HV/EV;
- transition to a deeper zone;
- crossing a configured next-add threshold;
- meaningful same-zone decline when configured;
- a deep-review trigger;
- a portfolio-wide cluster when configured.

Intraday low-zone touches may be preserved even if the price rebounds before the latest snapshot.

## Failure-safe behavior

- invalid valuation → `UNVALUED/NO_ZONE`;
- stale quote → no zone alert based on that quote;
- identity ambiguity → no classification;
- pending material review → show `REVIEW_PENDING` and do not imply the old valuation has been reconfirmed.
