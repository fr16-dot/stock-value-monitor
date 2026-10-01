# stock-value-monitor

A versioned, price-blind equity valuation and monitoring workflow.

## Structure

- `skill/fundamental-value-entry/SKILL.md` — core skill contract
- `skill/fundamental-value-entry/lessen_learnrd.md` — historical failures and permanent fixes
- `skill/fundamental-value-entry/references/registry-schema.md` — registry field contract
- `config/defaults.yaml` — default monitoring parameters

## Important provenance note

The original historical `SKILL.md` file was not recoverable from the accessible file store when this repository was created. The current skill was reconstructed from the latest validated operating rules and migrated registry configuration from the prior workflow.

The reconstruction preserves the key policies: price-blind/versioned valuation, TRADITIONAL vs HIGH_CONVEXITY modeling, RB/HV/EV zones, UNVALUED blocking, catalyst-forced review, quote-freshness and security-identity gates, same-zone re-alert thresholds, and recommendation-vs-execution separation.
