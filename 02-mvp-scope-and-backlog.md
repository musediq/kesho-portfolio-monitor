# Kesho Capital — MVP Scope and Backlog

The job for this sprint is to prove the model works for one real customer in time for a demo at the end of the sprint, without building anything the underlying data can't yet support. Two things follow from that.

First, the demo has one job: show Ada something that replaces the specific pain she described — assembling a holistic snapshot by hand in Excel for the board. Anything that doesn't serve that directly is a candidate for deferral, not because it lacks long-term value, but because building it now would mean building ahead of validated need.

Second, AfriCapital's operating constraint is ingestion time trending toward one day per customer, since the business model depends on low marginal cost per Pro customer. This sprint won't hit that target. The data is being hand-cleaned and mapped for a single customer, and that's expected — the job of sprint one is to validate the model is right. Automating ingestion is a later investment once the model has been proven across more than one customer.

## MVP Scope: In

- **Portfolio aggregation view**, cost and fair value, across all three position types, filtered to Active. This is the direct answer to Ada's ask for a single holistic snapshot.
- **Kesho's actual economic share for fund-of-funds positions**, not the GP-reported fund total. This is the single most important item in the MVP — it's the exact problem Ada called misleading.
- **Clean exit handling:** Craftbyte and Zolapay reclassified out of the active list.
- **Data-quality flags surfaced explicitly** rather than hidden: Greenwatt's zero value and SkillSprout's missing metrics marked "Needs Review."
- **MOIC where it's actually calculable today** — on the impact fund positions with real fair values, not forced onto positions where the invested-capital number isn't clean yet.

## MVP Scope: Deferred, and Why

**Cash-flow view (calls versus distributions):** deferred because the source data doesn't exist in usable form yet. Ada is still building the call schedule herself. Building a UI for data that isn't there yet means building ahead of it, not solving the actual sequencing.

**IRR:** downstream of the same blocker as the cash-flow view. A partial or estimated IRR would do more harm than showing none at all.

**Access control enforcement:** the data model already includes `AccessGrant`, so this isn't a redesign later, but enforcing it isn't necessary for a demo audience of two people. It needs to be built before more Kesho users are onboarded, not before the first demo.

**Sector comps and DCF support:** this is Chidi's ask, but it's a separate workstream tied to closing the sale, not something the Pro pod needs to solve to demonstrate value to Ada in this sprint.

**Automated ingestion pipeline:** this sprint proves the model works by hand, for one customer. Automating it is the right investment once the model has held up across a few customers, not before.

## Backlog

Estimates are in engineer-days, sized for planning conversation rather than a committed sprint plan.

### Epic 1: Data Foundation
| Ticket | Estimate |
|---|---|
| Implement `Position` supertype and `DirectPosition`, `FundCommitment`, `ImpactHolding` subtypes | 1.5 days |
| Implement `CashFlowEvent` schema — structure only, populated once the call schedule exists | 0.5 days |
| Ingest and map Kesho's seed data into the schema | 1.5 days |
| Reclassify Craftbyte and Zolapay to Exited, pending confirmation of exit date and proceeds | 0.5 days |

### Epic 2: Ownership and Valuation Logic
| Ticket | Estimate |
|---|---|
| Implement `kesho_ownership_pct` and `kesho_share_of_nav` derivation for fund commitments | 1.5 days |
| Surface the ownership-share assumption as a visible note in the data and UI | 0.5 days |
| MOIC calculation for positions with usable invested-capital data | 1 day |
| Data-quality flag logic | 0.5 days |

### Epic 3: Portfolio Snapshot View
| Ticket | Estimate |
|---|---|
| Aggregate view: total cost and fair value across position types, Active only | 1.5 days |
| Per-position breakdown table: sector, type, status, value | 1 day |
| "Needs Review" treatment for flagged data | 0.5 days |
| Recent-exits summary panel | 0.5 days |

### Epic 4: Demo Readiness
| Ticket | Estimate |
|---|---|
| Wire the clickable prototype — static or mocked data is acceptable | 1 day |
| Prepare a short talk track mapping each screen to what Ada specifically asked for | 0.5 days |

**Rough total for the in-scope MVP:** approximately 12 engineer-days, spread across a small pod, assuming data ingestion and ownership logic run in parallel with early UI work.

### Backlog — Visible, Not in This Sprint
| Item | Estimate | Blocked by |
|---|---|---|
| Capital-call schedule ingestion and net invested capital calculation | 2–3 days | Ada's call schedule doesn't exist yet |
| Cash-flow view | 1.5 days | Above |
| IRR calculation | 1.5 days | Above |
| Access control enforcement | 1–2 days | Not needed until more Kesho users onboard |
| Automated ingestion pipeline | Separate epic, sized later | Model needs validation across 2–3 customers first |

## Prioritization Summary

Everything in scope either fixes what Ada called misleading or broken — fund totals versus Kesho's share, exits left unreclassified — or makes the data trustworthy enough to act on, with visible flags instead of silent zeros. Everything deferred is deferred because the data it depends on doesn't exist yet, not because it lacks value.
