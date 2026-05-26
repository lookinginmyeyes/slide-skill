---
name: slide-data-visuals
description: >-
  Non-image chart generation module for single-file HTML slide decks. Use
  whenever a slide needs data visualization, Excel/CSV-derived charts, trend
  lines, bar/pie/scatter/heatmap/stacked charts, KPI cards, comparison tables,
  or dashboard pages. This module adapts Data Visuals data analysis, chart,
  infographic, and PPT slot-planning patterns into inline HTML/SVG/CSS/JS
  components that inherit the selected deck style, typography, background,
  motion, and viewport constraints.
---

# Slide Data Visuals

This module turns data into native slide components. It is the default path for
chart needs inside slides: when the content genuinely needs a chart, KPI system,
table visual, or dashboard view, call this module even if no image generation is
available. It does not create a separate dashboard app, export chart images by
default, or change the deck's visual identity.

Use it after source grounding / narrative planning and after the user has locked
the style direction and typography. The selected deck path still owns the
page skeleton; this module owns the chart grammar and dashboard slots inside
that skeleton.

## Hard Contract

- Output must be embeddable in the final single-file HTML deck.
- Treat this as **non-image chart generation**. Charts, KPI cards, tables, and
  dashboards should be generated as live/native HTML components, not image
  prompts and not PNG screenshots.
- Prefer inline SVG, semantic HTML/CSS, and small vanilla JS. If a chart library
  is already bundled or explicitly allowed, inline the required runtime or use a
  compact self-contained implementation. Do not require external network assets.
- Do not output standalone PNG chart images as the default. Use raster chart
  images only when the user explicitly asks for exportable images or when a
  source screenshot must be preserved as evidence.
- Charts and dashboards must inherit the deck's current tokens:
  `--bg`, `--surface`, `--ink`, `--muted`, `--accent`, `--accent-2`,
  `--grid`, `--border`, type scale, radius, shadow, and motion timing.
- Chart backgrounds should be transparent or tokenized surfaces, not a pasted
  white rectangle on a dark/editorial/magazine slide.
- Data colors must be derived from the selected style palette. Add chart-only
  series tokens by mixing the deck accent with neutrals; do not introduce a new
  rainbow palette unless the selected style already supports it.
- Typography must match the chosen typography preset. Use tabular numerals for
  axes, labels, KPI values, percentages, and animated numbers.
- Every chart must fit at 100% browser zoom and at 1280x720. Split overloaded
  dashboards across slides instead of shrinking labels into unreadability.

## What This Adapts From Data Visuals

Use Data Visuals as capability guidance, not as a separate style:

- `sn-da-excel-workflow`: row-count gate, large-file caution, cleaning before
  charting, group-by, pivot, time-series, KPI, and chart type selection.
- `sn-infographic`: dashboard layout, data-visualization clarity rules, visual
  completeness checks, and chart-specific failure rules.
- `sn-ppt-standard`: page slot planning, style-spec inheritance, chart container
  contracts, and "redesign around missing assets" discipline.

Do not import Data Visuals API config, image generation routes, CLI orchestration,
or multi-file PPT pipeline into this module.

## Input Contract

Accept any of these from earlier modules or the user:

```text
Data source:
Data shape:
Fields:
Measures:
Dimensions:
Time grain:
User question:
Slide job:
Selected deck style:
Selected typography:
Available token palette:
```

If the input is a file, coordinate with `slide-source-grounding` to extract only
the needed data. For large spreadsheets, follow the row-count and sampling
discipline in `references/data-visual-patterns.md`.

## Output Contract

Return a chart/dashboard spec that the main generator can embed:

```text
Slide:
Data message:
Visual form:
Data transform:
Fields used:
Component layout:
Token mapping:
Motion:
HTML/SVG implementation notes:
Accessibility labels:
Risks / data gaps:
```

For actual generation, the final HTML deck should contain the component markup,
styles, data arrays, and JS needed to render the visual without external files.

## Visual Form Selection

- Trend over time -> line / area / slope chart.
- Ranked categories -> horizontal bar, sorted descending unless a natural order
  matters.
- Composition -> stacked bar for change over time; donut/pie only for very few
  categories and only when parts sum to a meaningful whole.
- Two-variable relationship -> scatter with labels/callouts only for important
  points.
- Matrix / cross-tab / share by dimension -> heatmap or compact table with
  color scale.
- Few headline metrics -> KPI row or metric cards with one supporting sparkline.
- Many metrics at once -> dashboard grid, but cap at one primary chart, two to
  four KPI cards, and one secondary detail area per slide.
- Before/after -> paired bars, delta cards, or bridge/waterfall-style layout.

Avoid chart types that are decorative but low signal. If a chart does not make
the slide's claim easier to see, use a table, big number, or sentence instead.

## Style Integration

Before writing chart code, map chart tokens from the selected deck style:

```css
:root {
  --chart-bg: color-mix(in srgb, var(--surface) 88%, transparent);
  --chart-ink: var(--ink);
  --chart-muted: var(--muted);
  --chart-grid: color-mix(in srgb, var(--ink) 14%, transparent);
  --chart-axis: color-mix(in srgb, var(--ink) 28%, transparent);
  --chart-a: var(--accent);
  --chart-b: var(--accent-2);
  --chart-c: color-mix(in srgb, var(--accent) 55%, var(--ink));
  --chart-good: #2f9e67;
  --chart-warn: #c97721;
  --chart-bad: #c84646;
}
```

Adjust semantic colors to the deck palette when necessary. On dark decks, raise
line contrast and lower grid opacity. On editorial or Editorial Motion magazine decks,
let the animated background show through by using translucent chart surfaces.
On Swiss/data decks, use strict alignment, thin gridlines, and visible units.
On Warm Studio decks, use refined UI-panel styling but avoid nested cards.

## Motion

Coordinate with `slide-motion-polish`:

- Use chart draw-in, bar grow, line trace, dot reveal, heatmap fade, KPI count-up,
  or digit-roll only when it clarifies reading order.
- Keep chart motion inside stable containers. Animated values must not resize or
  push labels.
- Page transition numeric motion is encouraged for hero KPI slides.
- Dense executive dashboards should use restrained staggered reveals.
- Respect `prefers-reduced-motion`.

## Quality Gate

Fail and revise the component if:

- axes, units, or labels are missing when needed;
- series colors cannot be distinguished;
- chart background clashes with the slide background;
- labels require zooming;
- a dashboard has too many widgets for one slide;
- the chart uses fake or inferred data without marking it;
- legend is far from the data or consumes more attention than the chart;
- the component causes vertical overflow at 1280x720.

See `references/data-visual-patterns.md` and
`references/html-chart-components.md` only when the deck needs charts,
dashboards, KPI systems, or data-heavy table visuals.
