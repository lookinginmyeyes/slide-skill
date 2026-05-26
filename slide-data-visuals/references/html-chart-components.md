# Single-File HTML Chart Components

Use this when implementing charts inside the final HTML deck.

## Component Rules

- Use inline SVG for most charts: bar, line, area, scatter, heatmap, slope,
  waterfall-like bridge, sparkline, and KPI gauges.
- Use semantic HTML/CSS for KPI cards, metric grids, compact tables, legends,
  tabs, and dashboard frames.
- Use small vanilla JS only for scale calculation, count-up, digit roll, tooltip,
  or reveal timing.
- Store data as small inline arrays or JSON script blocks inside the HTML.
- Never require a network request to render the chart.

## CSS Token Pattern

```css
.chart-panel {
  background: var(--chart-bg);
  color: var(--chart-ink);
  border: 1px solid var(--border);
  border-radius: var(--radius-md, 8px);
}

.chart text {
  fill: var(--chart-muted);
  font-family: var(--font-sans);
  font-variant-numeric: tabular-nums;
}

.chart .axis,
.chart .grid {
  stroke: var(--chart-grid);
}

.chart .series-a { stroke: var(--chart-a); fill: var(--chart-a); }
.chart .series-b { stroke: var(--chart-b); fill: var(--chart-b); }
```

## Inline SVG Bar Chart Skeleton

```html
<figure class="chart-panel" aria-label="Revenue by segment">
  <figcaption>
    <strong>Enterprise accounts drive most of the growth</strong>
    <span>Revenue by segment, Q1-Q4</span>
  </figcaption>
  <svg class="chart bar-chart" viewBox="0 0 720 360" role="img">
    <g class="grid"></g>
    <g class="bars"></g>
    <g class="labels"></g>
  </svg>
  <p class="source">Source: user-provided spreadsheet</p>
</figure>
```

Keep the actual generated deck self-contained by expanding the groups into
concrete SVG elements or a tiny render function.

## Dashboard Layout Pattern

```html
<section class="dashboard-shell">
  <div class="kpi-row">
    <article class="kpi"><span>Revenue</span><strong data-count="128">128M</strong></article>
    <article class="kpi"><span>YoY</span><strong data-count="32">+32%</strong></article>
    <article class="kpi"><span>Retention</span><strong data-count="91">91%</strong></article>
  </div>
  <figure class="chart-panel primary-chart">...</figure>
  <aside class="insight-panel">...</aside>
</section>
```

Use CSS grid tracks with `minmax(0, 1fr)` and explicit max heights so dashboard
widgets do not push below the slide.

## Motion Hooks

```css
.slide.active .bar { animation: barGrow 520ms var(--ease-out) both; }
.slide.active .line-path { animation: lineTrace 680ms var(--ease-out) both; }
.slide.active .kpi strong { animation: digitLift 420ms var(--ease-out) both; }

@media (prefers-reduced-motion: reduce) {
  .bar, .line-path, .kpi strong { animation: none !important; }
}
```

Only use hooks that the selected deck motion system supports.

## ECharts-Style Guidance Without External Dependency

When thinking in ECharts terms, translate the concept into native components:

- `grid` -> chart inner margins
- `xAxis/yAxis` -> SVG axis and labels
- `series` -> SVG paths, rects, circles, or heatmap cells
- `tooltip` -> optional custom HTML tooltip
- `visualMap` -> legend and color scale

If ECharts is already embedded by the selected template or explicitly allowed,
set `renderer: 'svg'`, keep the container fixed-height, and derive the option's
colors, font family, background, gridline color, and tooltip style from deck
tokens.
