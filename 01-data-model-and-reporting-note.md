# Kesho Capital — Data Model and Reporting Note

Kesho's core reporting problem, based on the onboarding calls, is pretty simple to state and harder to fix: fund-level totals keep getting confused with Kesho's actual share, and committed capital keeps getting confused with called capital. That's a modeling problem before it's a UI problem. If the schema doesn't separate these cleanly, no dashboard sitting on top of it is going to get them right either. Three things from the call notes shaped how I built this:

- A fund's GP-reported NAV is never Kesho's number directly. Their share has to be derived.
- A capital call isn't the same as Kesho's invested capital. Fees and operating expenses need to come out using the actual call schedule, not a flat percentage.
- Exited positions need to be reclassified explicitly. Craftbyte and Zolapay are a good example of what happens when that doesn't happen.

## Core Entities

### 1. Client
The top-level tenant. One record for Kesho for now, but it extends to AfriCapital's other Pro customers.

**Fields:** `client_id`, `name`, `reporting_currency` (NGN), `inception_date`

### 2. Position (supertype)
Every economic exposure Kesho holds, regardless of type. This is what lets me pull an aggregated view across direct deals, fund stakes, and the impact fund as one query instead of three separate ones.

**Fields:** `position_id`, `client_id`, `position_type` (Direct / FundCommitment / ImpactHolding), `name`, `sector`, `status` (Active / Exited / Flagged), `currency`, `inception_date`, `exit_date`

Status includes a `Flagged` state alongside Active and Exited. Craftbyte and Zolapay are basically the reason this exists — divested but still sitting active in Kesho's own tracker. Flagged lets the system surface a likely reclassification without silently changing data Kesho hasn't actually confirmed.

### 3. DirectPosition (subtype of Position)
Greenline Rail, Anka Manufacturing, and the Coastal Free-Zone asset live here.

**Fields:** `position_id`, `basis` (Cost / MarkToMarket / RevenueMultiple), `stake_amount`, `co_investors`, `tradable`

### 4. FundCommitment (subtype of Position)
The LP stakes: Marara, Subira, Harmattan, Kanuri.

**Fields:** `position_id`, `manager_name`, `committed_amount`, `currency`, `fund_reported_nav`, `kesho_ownership_pct`, `kesho_share_of_nav`

**How ownership gets calculated:** `kesho_ownership_pct` is Kesho's committed capital divided by the total fund size, pulled from the LPA or GP statement where I can get it. `kesho_share_of_nav` is just that percentage applied to `fund_reported_nav`. I want to be upfront that this is an approximation. It assumes Kesho's economic share tracks their commitment percentage, which is usually true for standard LP structures but can drift if there's a side letter or a preferred-return waterfall involved. This is something to actually confirm with Kesho during onboarding — does it match their LPA terms, and do they have capital account statements that would give a real balance instead of an estimate.

### 5. ImpactHolding (subtype of Position)
Harvestbridge, Craftbyte, Zolapay, Greenwatt, SkillSprout. Since Kesho owns this fund outright, investment data doubles as financial reporting here, so I didn't need the ownership-share calculation that FundCommitment needs.

**Fields:** `position_id`, `portfolio_company`, `cost`, `fair_value`, `data_quality_flag` (Clean / ZeroOrPlaceholder / MissingMetrics)

### 6. CashFlowEvent
This is the entity that actually solves the flat-percentage problem.

**Fields:** `event_id`, `position_id`, `event_date`, `event_type` (Call / Distribution), `gross_amount`, `fee_component`, `opex_component`, `net_invested_capital`

Ada mentioned she's building a call schedule in Excel herself because the source data isn't captured cleanly right now. I designed this entity to be where that schedule lands once it exists. It doesn't need clean data on day one, it just needs somewhere for clean data to go when it's ready.

### 7. ValuationSnapshot
Point-in-time valuations, kept separate from the position itself so history sticks around instead of getting overwritten every quarter.

**Fields:** `snapshot_id`, `position_id`, `as_of_date`, `cost_basis`, `fair_value`, `source` (GPStatement / InternalMark / ManualEntry)

### 8. AccessGrant
Kesho mentioned not everyone on their team should see everything, especially on co-investments with confidential terms. So this needed to sit at the position level, not just the client level.

**Fields:** `user_id`, `position_id`, `visibility_level` (Full / Summary / None)

## Derived Metrics

- **MOIC:** `fair_value` divided by `net_invested_capital`, added up across a position's cash flow events.
- **DPI:** total distributions divided by `net_invested_capital`.
- **IRR:** calculated from the full cash flow time series plus current `fair_value` as a terminal cash flow.
- **Portfolio-level cost and fair value:** `DirectPosition.stake_amount` plus `FundCommitment.kesho_share_of_nav` plus `ImpactHolding.fair_value`, filtered to `Active` status.

## Reporting Note: How the Board Report Actually Gets Built From This

1. Pull every position where `status` equals Active. Once Craftbyte and Zolapay get reclassified, they just drop out of this view automatically — nobody has to remember to manually exclude them every quarter.
2. Aggregate at cost and fair value across all three position types. Direct positions at `stake_amount`, fund commitments at `kesho_share_of_nav` (not `fund_reported_nav`), impact holdings at `fair_value`. This one aggregation is basically the holistic snapshot Ada was asking for.
3. Build the cash-flow view from `CashFlowEvent`, split by `event_type`, using `net_invested_capital` instead of gross amounts so fees and expenses don't quietly inflate what looks like invested capital.
4. Compute MOIC, DPI, and IRR per position, then roll up to the portfolio level.
5. Put data-quality flags in the report instead of hiding them. Greenwatt's zero value and SkillSprout's missing metrics show up as "Needs Review," not as a silent zero sitting next to positions with real numbers.
6. Export or present the aggregated view.

## Things I'd Actually Ask Kesho During Onboarding

- Whether `kesho_ownership_pct` should be based on committed capital or an actual capital account balance — do capital account statements even exist?
- Exit dates and proceeds for Craftbyte and Zolapay, so DPI reflects them properly instead of just dropping them from the active list.
- What should happen to Greenwatt (zero/placeholder value) and SkillSprout (missing MOIC/DPI): write off, re-value, or leave pending until the source tracker gets fixed.
