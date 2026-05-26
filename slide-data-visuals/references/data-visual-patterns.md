# Data Visuals Data Patterns Adapted For Slides

Use this only for data-heavy slides.

## Data Discipline

- Count rows or estimate data scale before loading spreadsheets.
- For small data, read directly and inspect schema.
- For medium data, cache cleaned data before repeated transforms.
- For large data, sample first and use streaming / Parquet-style thinking rather
  than repeatedly loading entire sheets.
- Clean before charting: normalize text, handle merged-cell blanks with forward
  fill where semantically correct, convert numeric strings, and remove invalid
  rows.
- Preserve units, time period, source, and definitions.
- Mark gaps instead of inventing values.

## Analysis Patterns

- Group-by analysis: aggregate count, sum, mean, median, share, or rank by a
  chosen dimension.
- Pivot/crosstab: compare two categorical dimensions; use heatmap or compact
  matrix when both dimensions matter.
- KPI analysis: define numerator, denominator, unit, period, target, and delta.
- Time-series analysis: sort by date, check missing periods, show change rate
  only when the denominator is meaningful.
- Comparison analysis: align categories before comparing; avoid comparing totals
  with rates on the same axis.
- Distribution analysis: use histogram/box/scatter only when spread matters to
  the slide claim.

## Chart Selection

| Need | Best form | Avoid |
| --- | --- | --- |
| category ranking | horizontal bar | pie with many slices |
| time trend | line / area / slope | unordered bars |
| composition change | stacked bar / 100% stacked bar | 3D pie |
| two-category comparison | paired bars / delta card | overloaded table |
| cross dimension | heatmap / matrix | dense multi-series line |
| dashboard snapshot | KPI cards + one primary chart | six equal charts |
| outlier story | scatter + callout | unlabeled dots |

## Dashboard Slot Planning

A slide dashboard should have:

- one headline message;
- one primary chart;
- two to four KPI cards;
- one supporting table, heatmap, sparkline row, or callout panel;
- a tiny source/period note when needed.

If the dashboard needs more than that, split it into overview plus detail
slides. Do not create a wall of mini charts.

## Data Visuals Ideas To Keep

- Slot planning before page generation.
- Data integrity check before visual polish.
- Chart-specific critique: axes visible, series distinguishable, labels legible,
  layout balanced.
- Missing assets or missing data should trigger redesign, not fake placeholders.

## Data Visuals Ideas Not To Import

- Multi-file PPT pipeline.
- External CLI orchestration.
- API configuration.
- Standalone image-generation infographic workflow.
- Matplotlib image exports as the default slide chart path.
