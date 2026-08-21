# AfriCapital Pro — Kesho Capital Onboarding

This is my submission for the AfriCapital Pro PM III case study: preparing Kesho Capital's first onboarding-sprint demo, a plan for building it, and a way of knowing afterward whether it actually worked.

**Live prototype:** [`https://musediq.github.io/kesho-portfolio-monitor/AfriCapital_Pro__standalone_.html`](https://musediq.github.io/kesho-portfolio-monitor/AfriCapital_Pro__standalone_.html)

Open the link above to see it running. The raw file below is large and minified, so GitHub's own file preview can fail to render it — that's a GitHub viewer limitation, not a problem with the prototype itself.

## What's in here

| # | Deliverable | File |
|---|---|---|
| 1 | Working prototype | [`AfriCapital_Pro__standalone_.html`](./AfriCapital_Pro__standalone_.html) — use the live link above, not this one, to actually view it |
| 2 | Portfolio data | [`Kesho-seed-data.xlsx`](./Kesho-seed-data.xlsx) |
| 3 | Data model and reporting note | [`01-data-model-and-reporting-note.md`](./01-data-model-and-reporting-note.md) |
| 4 | MVP scope and backlog | [`02-mvp-scope-and-backlog.md`](./02-mvp-scope-and-backlog.md) |
| 5 | Success and instrumentation plan | [`03-success-and-instrumentation-plan.md`](./03-success-and-instrumentation-plan.md) |

## Why I built it in this order

I started with the data model, before touching anything else. Kesho's actual problem, fund totals getting confused with their real share, committed capital getting confused with called capital, is a modeling problem before it's a UI problem. If I'd started with the prototype, I'd have been styling a dashboard that was still wrong underneath.

Once the model was right, the MVP scope followed pretty naturally from it. What's in scope fixes what Ada actually called misleading. What's deferred is deferred because the data behind it doesn't exist yet, not because it doesn't matter.

The instrumentation plan came next, because I wanted to know what "working" would even look like before I built the thing that was supposed to work.

The seed data and prototype came last on purpose. Both are fast to put together once the model and scope are locked, and building them first is usually how people end up polishing the wrong thing.

One assumption I want to flag upfront rather than let someone find it: Kesho's ownership percentage in each fund is estimated as committed capital divided by total fund size, applied to the fund's reported NAV. That's a modeled approximation, not something the case gave me directly, and I've said so in the data model note and in the prototype itself. The real next step would be confirming it against Kesho's actual capital account statements.

## A note on the data

Figures are illustrative. I extended the case study's original seed data to be internally consistent and to actually demonstrate the reporting logic working end to end. Where the case didn't specify something, fund sizes, the FX rate, a couple of exit dates, I made a reasonable assumption and labeled it as one, both in the workbook and in the data model note.
