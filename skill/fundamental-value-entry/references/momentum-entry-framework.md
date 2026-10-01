# Momentum Entry Framework

Momentum is an optional execution overlay used only after intrinsic value is locked.

## Allowed inputs

- verified catalyst quality;
- revision in forward revenue/margins/FCF or milestone probability;
- event open/high/low/close;
- gap behavior;
- relative volume;
- market- and sector-relative strength;
- defined event-failure level.

## Status

Use qualitative statuses such as:

- `CHASE_QUALIFIED`
- `CONDITIONAL`
- `WAIT`
- `NO_CHASE`
- `FAILED`

Do not hard-code portfolio weights or tranche percentages in the valuation skill. Position sizing belongs to execution policy or explicit user context.

## Boundary

Momentum can change timing and implementation. It cannot change intrinsic value, model class, or RB/HV/EV zones.
