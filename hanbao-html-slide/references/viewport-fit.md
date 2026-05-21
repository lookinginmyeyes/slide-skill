# Viewport Fit Reference

Use this when a deck has dense pages, big typography, architecture diagrams,
tables, terminal panels, image grids, metric strips, or any slide that may not
fit in a normal browser viewport.

## Core Requirement

The user must not need to zoom the browser out to see the whole slide.

Validate at:

- 16:9 full screen
- 1280x720
- normal desktop browser window with visible browser chrome
- low-height desktop viewport around 720-820px

## CSS Pattern

```css
.slide {
  height: 100vh;
  overflow: hidden;
  padding: clamp(28px, 3.6vw, 52px);
  display: grid;
  grid-template-rows: auto minmax(0, 1fr) auto;
}

.stage {
  align-self: center;
  max-height: calc(100vh - clamp(104px, 12vh, 140px));
}

@media (max-height: 820px) and (min-width: 921px) {
  .slide { padding: 24px 42px; }
  h1 { font-size: clamp(52px, 7.4vw, 104px); }
  h2 { font-size: clamp(31px, 4.35vw, 58px); }
  .dense-row { min-height: 42px; padding: 7px 12px; }
  .metric-card, .flow-card, .visual-card { min-height: auto; padding: 12px; }
  .diagram { min-height: min(330px, 52vh); }
}
```

## Component Rules

- Tables: keep rows short, compact row padding, or split into two slides.
- Metric strips: cap card height and reduce number size in low-height mode.
- Terminal panels: cap height and reduce line-height in low-height mode.
- Architecture diagrams: use `min-height: min(<px>, <vh>)`.
- Flow cards: use short copy and lower `min-height` in low-height mode.
- Closing quote pages: use a compact heading class when the quote is long.

## Audit Questions

- Can I see the footer/progress without zooming out?
- Can I see the last row of the densest table?
- Is the title still readable after compacting?
- Would splitting the slide preserve quality better than shrinking more?
