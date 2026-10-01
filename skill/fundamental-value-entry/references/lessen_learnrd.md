# Monitoring lessons learned

Read this file before watchlist monitoring, missed-opportunity audits, database repairs, and scheduler changes. Treat every incident below as a regression test.

## Invariants

1. Every active ticker must have exactly one ACTIVE valuation or an explicit non-corporate monitoring model. Missing and duplicate records are errors, not silent exclusions.
2. `UNVALUED`/`NO_ZONE` is the only valid zone state without an ACTIVE valuation. Never default to `ABOVE_RB`.
3. Adding a ticker must atomically create Stocks, provisional ValuationVersions, MarketState, and ActionState records. A watchlist addition is incomplete until all four exist.
4. A material catalyst bypasses EXTENDED-tier lightweight screening and triggers a full review even when valuation is missing.
5. Validate quote symbol, exchange, currency, session, timestamp, and freshness before zone mapping. Stale quotes cannot suppress alerts.
6. Daily HV/EV enumeration and FOCUS scans are completeness checks, not optional summaries. Duplicate suppression cannot hide a deeper zone, a next-add crossing, a new trade date, or a portfolio cluster.
7. Recommended and executed states remain separate. Never infer a position from an unconfirmed suggestion.
8. After a write, re-read and verify: one ACTIVE version, valid fixed bands, correct zone, next trigger, event key, and unchanged user-confirmed fields.
9. A ticker is not a durable issuer identifier. Verify legal entity, exchange, currency, asset type, and CIK/issuer identifier before reusing history, filings, or valuation bands.

## Incident register

### 2026-07-29 — high-value names not alerted

- Failure: several names entered high-value areas without an alert.
- Causes: incomplete cross-sectional scanning and duplicate suppression that operated ticker by ticker.
- Control: enumerate all active HV/EV names once per U.S. trading day and run portfolio-cluster logic independently of per-ticker deduplication.

### 2026-08 to 2026-09 — ONTO, TSSI, LUNR and related missed entries

- Failure: valid price windows or fixed-zone contacts were observed after the fact.
- Causes: incomplete valuation migration, inconsistent next-trigger state, and treating missing records as outside the opportunity set.
- Control: perform the active-watchlist minus exact-one-ACTIVE audit before every scan; create provisional valuations instead of excluding names.

### 2026-09-21 — HV and EV presentation was merged

- Failure: output combined the two priority sections, obscuring severity.
- Cause: report formatting did not enforce separate buckets.
- Control: always render independent EV then HV sections, including an explicit “none” row for an empty section.

### 2026-09-22 — HPS.A stale Canadian quote

- Failure: an old C$240.30 quote was repeatedly carried forward and a material expansion catalyst was not surfaced promptly.
- Causes: U.S.-centric quote refresh, no exchange-session freshness test, and insufficient escalation of material news for a stale-priced FOCUS name.
- Control: validate TSX symbol/exchange/session independently; mark stale data; search material company news even when a fresh quote is unavailable; never use stale price to conclude no trigger.

### 2026-09-22 — VICR near US$200 was not evaluated or alerted

- Failure: VICR was active but had no ACTIVE or provisional valuation, no ReviewLog, no next trigger/add, and no alert. Its MarketState was nevertheless labeled `ABOVE_RB`. A material patent-licensing/guidance catalyst also failed to escalate.
- Causes: EXTENDED names were allowed to remain `UNVALUED`; zone code defaulted missing bands to `ABOVE_RB`; event screening depended too heavily on price direction and valuation readiness.
- Control: create a provisional valuation when a ticker is added; represent missing valuation as `UNVALUED`/`NO_ZONE`; make material guidance/licensing events valuation-independent escalation triggers; audit and repair all active missing valuations.

### 2026-09-22 — AKTS ticker identity had changed

- Failure risk: the watchlist symbol could have been analyzed as the former Akoustis Technologies even though the ticker had been reused by Aktis Oncology after the former issuer's bankruptcy and asset sale.
- Causes: symbol-only identity matching and blank exchange metadata.
- Control: bind every ticker to current issuer identity before valuation; treat ticker reuse, rebrands, mergers, ADR-ratio changes, and exchange transfers as material identity events; never carry the predecessor's fundamentals or fixed zones into the successor.

## Required regression checks

- `active tickers - tickers with exactly one ACTIVE valuation = 0`, except explicitly modeled indices where an ACTIVE monitoring model must still exist.
- No `UNVALUED` ticker has `ABOVE_RB`, `RB`, `HV`, or `EV` in MarketState.
- Every FOCUS quote is fresh for its own exchange or explicitly marked stale.
- Every material event found in IR/regulatory sources has a ReviewLog entry and momentum disposition.
- Every generated alert key contains ticker, valuation version, trigger type, and trigger level; daily HV/EV keys also contain trade date.
- No write changes `user_action`, `confirmed_shares`, or `average_cost` without user confirmation.
- Every active ticker resolves to the intended current issuer/security; ticker reuse or corporate-action changes have a ReviewLog entry before valuation.
