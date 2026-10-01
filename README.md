# stock-value-monitor

A versioned, price-blind equity valuation and monitoring workflow.

## Canonical skill package

The validated skill is stored under `skill/fundamental-value-entry/`:

- `SKILL.md` — core valuation, momentum, monitoring, and execution workflow
- `agents/openai.yaml` — skill UI metadata
- `assets/icon.svg` — skill icon
- `references/lessen_learnrd.md` — incident-derived controls and permanent fixes
- `references/momentum-entry-framework.md` — catalyst and momentum-entry rules
- `references/valuation-record-template.md` — versioned valuation record schema/template
- `references/watchlist-monitoring-framework.md` — full-list scanning, trigger, and deployment rules

The package is copied from the current validated installed skill. `config/defaults.yaml` contains optional repository-level defaults and is not part of the installable skill directory.

## Scope note

The skill defines the reusable analytical workflow. Live schedules, Airtable records, portfolio holdings, alert state, and automation prompts are deployment-specific runtime state and are not embedded in the skill package.
