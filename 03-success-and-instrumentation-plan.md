# Kesho Capital — Success and Instrumentation Plan

Success here isn't the demo landing well. It's Kesho no longer needing Excel to produce a board report. Those are different bars, and it's worth being explicit that this plan tracks toward the second one, checked at three points: the demo itself, the weeks of onboarding that follow, and the first full quarterly cycle where Pro either becomes the system of record or doesn't.

## What Success Looks Like at Each Stage

**Demo day:** Ada and Chidi can see their own actual numbers, correctly split between fund totals and Kesho's real share, with Craftbyte and Zolapay properly reclassified. The bar isn't a polished interface. It's Ada looking at it and recognizing her own portfolio, correctly, for the first time in a tool instead of a spreadsheet.

**Onboarding weeks (the weekly check-ins Ada asked for):** Ada is opening the tool between check-ins, not just during them, and the number of things she's manually correcting or flagging is trending down week over week rather than staying flat.

**Steady state, first full quarter:** the board report gets produced from AfriCapital Pro directly, not assembled by hand afterward with the tool's numbers as a side reference.

## Outcome Metrics

These are the numbers that actually indicate the problem is solved, not just that the tool is being opened.

| Metric | How it's measured | Cadence |
|---|---|---|
| Board report produced directly from Pro vs. rebuilt in Excel | Qualitative confirmation from Ada each quarter | Quarterly |
| Time to assemble the quarterly snapshot | Self-reported by Ada, compared against her stated baseline of manual assembly | Quarterly |
| Open data-quality flags at time of board reporting | Count of positions still marked Flagged or Needs Review when the report is due | Quarterly, checked weekly leading up to it |

## Adoption Signals — Leading Indicators During Onboarding

These are what tell the story before the quarterly outcome numbers are even available.

| Signal | What it tells us |
|---|---|
| Session frequency | Ada opening the tool between scheduled check-ins, not just during them |
| Manual overrides trending down | Fewer corrections to position status, valuations, or ownership figures week over week, as the data converges toward accuracy |
| Flag resolution rate | Needs Review items getting resolved rather than accumulating unresolved |
| Direct reference in board materials | Whether Ada is pulling numbers straight from the tool into the actual board deck, confirmed by asking her directly rather than inferring it |

## Regression Signals — Quietly Gone Back to Excel

A drop in usage alone is ambiguous — it could mean the data is finally clean and needs less attention, or it could mean Ada has given up and moved back to her old workbook. These signals, especially in combination, distinguish the two.

- No login activity for a full week during active onboarding, particularly close to a reporting deadline.
- Repeated requests for raw data exports rather than using the aggregated views — a proxy for rebuilding the analysis elsewhere.
- A rise in ad-hoc questions to the pod directly instead of Ada finding answers in the tool herself, which usually signals a trust or usability gap rather than a data gap.
- Chidi requesting a manual reconciliation before taking numbers to the steering committee. This is a distinct signal from Ada's day-to-day usage — it means the numbers aren't yet trusted at the decision-making level even if the daily workflow looks fine.

## What I'd Do Next, Based on What I See

**If usage drops in the two weeks before a board meeting:** treat it as urgent, not something to raise at the next scheduled check-in. Reach out directly and find out whether it's a data problem, a trust problem, or a usability problem — those need different fixes.

**If manual overrides cluster around the ownership-percentage calculation specifically:** that's a signal the committed-capital approximation isn't holding up against Kesho's actual fund terms, and it's worth going back for capital account statements rather than continuing to patch the estimate.

**If raw-data export requests spike:** don't assume it's disengagement without checking — ask Ada directly what she's doing with the export. It might be a legitimate need the tool doesn't serve yet, not necessarily a sign of abandonment.

**If Chidi asks for a parallel manual reconciliation:** treat this separately from Ada's metrics entirely, since it points at a different layer of trust — the numbers may be operationally fine but not yet credible enough for a committee decision.

## What's Deliberately Not Instrumented This Sprint

Cross-customer benchmarking — comparing Kesho's onboarding-to-trust timeline against future Pro customers — isn't meaningful yet with a single customer. That becomes useful once two or three more are onboarded and there's a baseline to compare against, not before.
