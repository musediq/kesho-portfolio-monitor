# AfriCapital Pro — Kesho Capital Onboarding

Case study submission for the AfriCapital Pro PM III case: preparing Kesho Capital's first onboarding-sprint demo, a scoped plan to build it, and a system for knowing whether it actually worked.

**Live prototype:** `https://<your-github-username>.github.io/<repo-name>/` — once GitHub Pages is enabled (see below), this link goes live at the URL above.

## What's here

| # | Deliverable | File |
|---|---|---|
| 1 | Working prototype | [`/docs/index.html`](./docs/index.html) — also live via GitHub Pages, see link above |
| 2 | Realistic portfolio data | [`kesho-seed-data.xlsx`](./kesho-seed-data.xlsx) — six linked sheets, fully formula-driven, zero hardcoded totals |
| 3 | Data model and reporting note | [`01-data-model-and-reporting-note.md`](./01-data-model-and-reporting-note.md) |
| 4 | MVP scope and backlog | [`02-mvp-scope-and-backlog.md`](./02-mvp-scope-and-backlog.md) |
| 5 | Success and instrumentation plan | [`03-success-and-instrumentation-plan.md`](./03-success-and-instrumentation-plan.md) |

## How these five fit together

The data model came first — Kesho's actual problem (fund totals confused with their real share, committed capital confused with called capital) is a modeling problem before it's a UI problem. The MVP scope follows directly from the model: what's in scope fixes what Ada called misleading or broken; what's deferred is deferred because the underlying data doesn't exist yet, not because it lacks value. The instrumentation plan defines what "working" means before the thing that's supposed to work gets built. The seed data and prototype came last, once the model and scope were locked — every number in the workbook and the prototype ties out exactly to the same figures, cross-checked programmatically, not by eye.

The one modeled assumption worth flagging up front: Kesho's ownership percentage in each fund is estimated as committed capital ÷ total fund size, applied to the GP-reported NAV. That's a stated approximation, documented in the data model note and visible directly in the prototype's fund panels — not a fact, and the right next step is confirming it against Kesho's actual capital account statements.

## Enabling the live demo link

This repo is set up so GitHub Pages can serve the prototype straight from `/docs` with no build step:

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Branch: `main`, folder: `/docs`. Save.
5. The live URL appears at the top of that same settings page within a minute or two, and matches the pattern at the top of this README.

## A note on the data

Figures are illustrative, extended from the case study's original seed data to be internally consistent and demonstrate real reporting logic — fund sizes, the FX rate, and specific dates were modeled assumptions where the source material didn't specify them, and are labeled as such in both the workbook and the data model note.
