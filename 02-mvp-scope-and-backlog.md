# Kesho Capital — MVP Scope and Backlog

The job for this sprint is to prove the model actually works for one real customer, in time for a demo at the end of the sprint, without building anything the data can't honestly support yet. Two things follow from that.

First, the demo has one job: show Ada something that replaces the specific pain she described — assembling a holistic snapshot by hand in Excel for the board. If something doesn't serve that directly, it's a candidate for deferral. Not because it's not valuable eventually, but because building it now means building ahead of anything validated.

Second, AfriCapital's real constraint is ingestion time trending toward one day per customer, since the whole business model depends on onboarding being cheap. This sprint isn't going to hit that. The data's being hand-cleaned and mapped for a single customer right now, and honestly that's fine — the job of sprint one is just to prove the model is right. Automating ingestion is a later investment, once the model's held up across more than one customer.

## What's In

- **Portfolio aggregation view**, cost and fair value, across all three position types, Active only. This is the direct answer to what Ada asked for.
- **Kesho's actual economic share for fund-of-funds positions**, not the GP-reported fund total. Honestly the single most important thing in here — it's the exact thing Ada called misleading.
- **Clean exit handling:** Craftbyte and Zolapay come out of the active list.
- **Data-quality flags shown explicitly**, not hidden: Greenwatt's zero value and SkillSprout's missing metrics marked "Needs Review."
- **MOIC where it's actually calculable right now** — on the impact fund positions with real fair values, not forced onto positions where the invested-capital number isn't clean yet.

## What's Deferred, and Why

**Cash-flow view (calls vs. distributions):** deferred because the source data just doesn't exist in usable form. Ada's still building the call schedule herself. Building a screen for data that isn't there yet isn't solving the sequencing problem, it's ignoring it.

**IRR:** blocked by the same thing as the cash-flow view. A partial or guessed-at IRR would honestly do more damage than not showing one.

**Access control enforcement:** the model already has `AccessGrant` built in, so this isn't a redesign down the line, but enforcing it isn't necessary for a demo with an audience of two people. Needs to exist before more Kesho users get added, not before the first demo.

**Sector comps and DCF support:** this is Chidi's ask, but it's really a separate workstream tied to closing the deal, not something the Pro pod needs to solve to show Ada value this sprint.

**Automated ingestion pipeline:** this sprint proves the model works by hand, for one customer. Automating it is the right call once it's held up across a few customers, not before that.

## Backlog

Estimates are engineer-days, sized for a planning conversation, not a locked sprint commitment.

### Epic 1: Data Foundation
| Ticket | Estimate |
|---|---|
| Implement `Position` supertype and `DirectPosition`, `FundCommitment`, `ImpactHolding` subtypes | 1.5 days |
| Implement `CashFlowEvent` schema — structure only, gets populated once the call schedule exists | 0.5 days |
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
| Wire the clickable prototype — static or mocked data is fine | 1 day |
| Prepare a short talk track mapping each screen to what Ada actually asked for | 0.5 days |

**Rough total for the in-scope MVP:** around 12 engineer-days, across a small pod, assuming data ingestion and ownership logic run alongside early UI work rather than after it.

### Backlog — Visible, But Not This Sprint
| Item | Estimate | Blocked by |
|---|---|---|
| Capital-call schedule ingestion and net invested capital calculation | 2–3 days | Ada's call schedule doesn't exist yet |
| Cash-flow view | 1.5 days | Above |
| IRR calculation | 1.5 days | Above |
| Access control enforcement | 1–2 days | Not needed until more Kesho users onboard |
| Automated ingestion pipeline | Separate epic, sized later | Model needs proving across 2–3 customers first |

## Why It's Scoped This Way

Everything in scope fixes something Ada actually called misleading or broken — fund totals versus Kesho's real share, exits sitting unreclassified — or makes the data trustworthy enough to act on, with visible flags instead of silent zeros. Everything deferred is deferred because the data behind it doesn't exist yet. Not because it doesn't matter.
