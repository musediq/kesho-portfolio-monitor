# Kesho Capital — Success and Instrumentation Plan

Success here isn't the demo going well. It's Kesho not needing Excel anymore to produce a board report. Those are genuinely different bars, and I want to be upfront that this plan is tracking toward the second one, checked at three points: the demo itself, the weeks of onboarding after it, and the first full quarterly cycle where Pro either becomes the actual system of record or it doesn't.

## What Success Looks Like at Each Stage

**Demo day:** Ada and Chidi can see their own real numbers, correctly split between fund totals and Kesho's actual share, with Craftbyte and Zolapay properly reclassified. The bar isn't a polished interface — it's Ada looking at it and recognizing her own portfolio, correctly, for the first time in a tool instead of a spreadsheet.

**Onboarding weeks (the weekly check-ins Ada asked for):** Ada's opening the tool between check-ins, not just during them, and the number of things she's manually correcting or flagging is going down week over week instead of staying flat.

**Steady state, first full quarter:** the board report actually comes out of AfriCapital Pro directly. Not assembled by hand afterward with the tool's numbers used as a side reference.

## Outcome Metrics

These are the numbers that tell you whether the actual problem got solved, not just whether people opened the tool.

| Metric | How it's measured | Cadence |
|---|---|---|
| Board report produced directly from Pro vs. rebuilt in Excel | Qualitative confirmation from Ada each quarter | Quarterly |
| Time to assemble the quarterly snapshot | Self-reported by Ada, compared against her stated baseline for manual assembly | Quarterly |
| Open data-quality flags at time of board reporting | Count of positions still marked Flagged or Needs Review when the report's due | Quarterly, checked weekly leading up to it |

## Adoption Signals — What Tells the Story Early

These are the things that show up before the quarterly outcome numbers even exist.

| Signal | What it tells us |
|---|---|
| Session frequency | Ada opening the tool between scheduled check-ins, not just during them |
| Manual overrides trending down | Fewer corrections to status, valuations, or ownership figures week over week, as the data settles into being accurate |
| Flag resolution rate | Needs Review items actually getting resolved instead of piling up |
| Direct reference in board materials | Whether Ada's pulling numbers straight from the tool into the real board deck — confirmed by just asking her, not by guessing |

## Regression Signals — Quietly Back to Excel

A drop in usage on its own doesn't tell you much. It could mean the data's finally clean and needs less babysitting, or it could mean Ada's given up and gone back to her old workbook. These signals, especially together, are what actually distinguish the two.

- No login activity for a full week during active onboarding, especially close to a reporting deadline.
- Repeated requests for raw data exports instead of using the aggregated views — usually means she's rebuilding the analysis somewhere else.
- More ad-hoc questions coming to the pod directly instead of Ada finding answers in the tool herself. Usually a trust or usability problem, not a data problem.
- Chidi asking for a manual reconciliation before he takes numbers to the steering committee. This one's separate from Ada's day-to-day usage — it means the numbers aren't trusted at the decision-making level yet, even if the daily workflow looks fine.

## What I'd Actually Do With These Signals

**If usage drops in the two weeks before a board meeting:** treat it as urgent, not something to bring up at the next scheduled check-in. Reach out directly and figure out if it's a data problem, a trust problem, or a usability problem — those all need different fixes.

**If manual overrides cluster around the ownership-percentage calculation specifically:** that's telling me the committed-capital approximation isn't holding up against Kesho's actual fund terms, and it's worth going back for real capital account statements instead of keep patching the estimate.

**If raw-data export requests spike:** I wouldn't assume disengagement without checking first. Just ask Ada what she's doing with the export — might be a legitimate need the tool doesn't cover yet, not necessarily someone giving up on it.

**If Chidi asks for a parallel manual reconciliation:** treat this completely separately from Ada's metrics, since it's about a different layer of trust — the numbers might be operationally fine but just not credible enough yet for a committee decision.

## What I'm Deliberately Not Tracking This Sprint

Cross-customer benchmarking — comparing Kesho's onboarding-to-trust timeline against future Pro customers — doesn't mean anything with just one customer. That becomes useful once two or three more are onboarded and there's actually something to compare against.
