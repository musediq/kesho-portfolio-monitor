# Kesho Capital — Data Model and Reporting Note

Kesho's core reporting problem, based on the onboarding calls, is that fund-level totals get confused with Kesho's actual share, and committed capital gets confused with called capital. That's a data modeling problem before it's a UI problem: if the model doesn't separate these cleanly at the schema level, no dashboard built on top of it will get them right either. Three rules from the call notes shaped this model:

- A fund's GP-reported NAV is never Kesho's number directly. Their share has to be derived.
- A capital call is never the same as Kesho's invested capital. Fees and operating expenses need to be stripped out using the actual call schedule, not a flat percentage.
- Exited positions need to be reclassified explicitly. Craftbyte and Zolapay show what happens when that doesn't happen.

## Core Entities

### 1. Client
The top-level tenant. One record for Kesho today, extensible to AfriCapital's other Pro customers.

**Fields:** `client_id`, `name`, `reporting_currency` (NGN), `inception_date`

### 2. Position (supertype)
Every economic exposure Kesho holds, regardless of type. This is what lets an aggregated view across direct deals, fund stakes, and the impact fund run as a single query instead of three.

**Fields:** `position_id`, `client_id`, `position_type` (Direct / FundCommitment / ImpactHolding), `name`, `sector`, `status` (Active / Exited / Flagged), `currency`, `inception_date`, `exit_date`

Status includes a `Flagged` state alongside Active and Exited. Craftbyte and Zolapay are the exact case this solves — divested but not yet reclassified in Kesho's own tracker. Flagged lets the system surface a likely reclassification without silently changing data Kesho hasn't confirmed.

### 3. DirectPosition (subtype of Position)
Covers Greenline Rail, Anka Manufacturing, and the Coastal Free-Zone asset.

**Fields:** `position_id`, `basis` (Cost / MarkToMarket / RevenueMultiple), `stake_amount`, `co_investors`, `tradable`

### 4. FundCommitment (subtype of Position)
Covers the LP stakes: Marara, Subira, Harmattan, Kanuri.

**Fields:** `position_id`, `manager_name`, `committed_amount`, `currency`, `fund_reported_nav`, `kesho_ownership_pct`, `kesho_share_of_nav`

**Ownership calculation:** `kesho_ownership_pct` is Kesho's committed capital divided by total fund size, taken from the LPA or GP statement where available. `kesho_share_of_nav` is that percentage applied to `fund_reported_nav`. This is a known approximation — it assumes Kesho's economic share tracks its commitment percentage, which holds for standard LP structures but can drift with a side letter or preferred-return waterfall. This should be confirmed with Kesho directly during onboarding: whether it matches their LPA terms, and whether they have capital account statements that would give an actual balance instead of an estimate.

### 5. ImpactHolding (subtype of Position)
Covers Harvestbridge, Craftbyte, Zolapay, Greenwatt, and SkillSprout. Because Kesho owns the fund outright, investment data doubles as financial reporting here, so this subtype doesn't need an ownership-share calculation the way FundCommitment does.

**Fields:** `position_id`, `portfolio_company`, `cost`, `fair_value`, `data_quality_flag` (Clean / ZeroOrPlaceholder / MissingMetrics)

### 6. CashFlowEvent
This is what solves the flat-percentage problem directly.

**Fields:** `event_id`, `position_id`, `event_date`, `event_type` (Call / Distribution), `gross_amount`, `fee_component`, `opex_component`, `net_invested_capital`

Ada mentioned she's building a call schedule in Excel because the source data isn't captured cleanly today. This entity is designed as the destination for that schedule once it exists — it doesn't need clean data on day one, it needs a place for clean data to land when it's ready.

### 7. ValuationSnapshot
Point-in-time valuations, kept separate from the position itself so history is preserved rather than overwritten each quarter.

**Fields:** `snapshot_id`, `position_id`, `as_of_date`, `cost_basis`, `fair_value`, `source` (GPStatement / InternalMark / ManualEntry)

### 8. AccessGrant
Kesho noted that not everyone on their team should see everything, particularly on co-investments with confidential terms. This needs to be position-level, not just client-level.

**Fields:** `user_id`, `position_id`, `visibility_level` (Full / Summary / None)

## Derived Metrics

- **MOIC:** `fair_value` divided by `net_invested_capital`, aggregated across a position's cash flow events.
- **DPI:** total distributions divided by `net_invested_capital`.
- **IRR:** calculated from the full cash flow time series plus current `fair_value` as a terminal cash flow.
- **Portfolio-level cost and fair value:** the sum of `DirectPosition.stake_amount`, `FundCommitment.kesho_share_of_nav`, and `ImpactHolding.fair_value`, filtered to `Active` status.

## Reporting Note: How the Board Report Gets Built From This Model

1. Pull all positions where `status` equals Active. Once Craftbyte and Zolapay are reclassified, they drop out of this view automatically instead of needing to be manually excluded every quarter.
2. Aggregate at cost and at fair value across all three position types — direct positions at `stake_amount`, fund commitments at `kesho_share_of_nav` rather than `fund_reported_nav`, impact holdings at `fair_value`. This single aggregation produces the holistic snapshot Ada asked for.
3. Build the cash-flow view from `CashFlowEvent`, split by `event_type`, using `net_invested_capital` rather than gross amounts so fees and expenses don't inflate what looks like invested capital.
4. Compute MOIC, DPI, and IRR per position and rolled up to the portfolio level.
5. Surface data-quality flags in the report rather than hiding them. Greenwatt's zero value and SkillSprout's missing metrics appear as "Needs Review," not as a silent zero next to positions with real numbers.
6. Export or present the aggregated view.

## Open Questions for Kesho During Onboarding

- Whether `kesho_ownership_pct` should be based on committed capital or an actual capital account balance — ask if capital account statements exist.
- Confirm exit dates and proceeds for Craftbyte and Zolapay so DPI reflects them correctly, not just removes them from the active list.
- Confirm what should happen to Greenwatt (zero or placeholder value) and SkillSprout (missing MOIC/DPI): write off, re-value, or hold as pending until the source tracker is fixed.
