# Hanbao Slide Skills

Hanbao Slide Skills is a single uploadable skill package for producing polished,
single-file HTML slide decks. It keeps the package modular internally while
exposing one root `SKILL.md` for installation and routing.

## What It Does

- Generates self-contained HTML slide decks.
- Supports direct generation, style selection, and template/case selection.
- Auto-selects typography for direct generation; asks for typography after a
  concrete style or case choice.
- Uses source grounding and narrative planning when the input needs them.
- Uses inline data visuals for chart, KPI, table, and dashboard-style slides.
- Uses generated visual assets when image generation is available and the user
  has not opted out.
- Runs motion polish and visual audit as default quality gates.

## Referenced And Distilled Capabilities

This package adapts useful ideas from several local/open skill patterns:

- `html-ppt` / studio deck patterns: single-file deck runtime, layouts, themes,
  navigation, and presentation controls.
- Editorial motion slide patterns: magazine/editorial layouts, Swiss-style
  structure, WebGL dual-background treatment, and cinematic section rhythm.
- Frontend slide patterns: animation-rich HTML presentation craft, viewport
  fitting, and style-preview thinking.
- Warm high-fidelity HTML design patterns: typography feel, visual anchors, and
  single-file-safe design execution.
- Source grounding patterns: compact evidence extraction from documents,
  folders, PDFs, DOCX, spreadsheets, and local knowledge material.
- Narrative planning patterns: slide jobs, action titles, click beats, evidence
  hierarchy, and story rhythm.
- Image generation patterns: asset role planning, prompt strengthening, and
  environment-provided image generation.
- Visual audit patterns: hierarchy, density, layout rhythm, typography, color,
  responsive behavior, and anti-template checks.
- Motion design patterns: page transitions, entry choreography, numeric motion,
  chart draw-ins, interaction feedback, and reduced-motion support.

## Package Structure

```text
SKILL.md
ROUTING.md
hanbao-html-slide/
slide-source-grounding/
slide-narrative-planner/
slide-data-visuals/
slide-visual-assets/
slide-typography/
slide-visual-audit/
slide-motion-polish/
```

## Design Principle

Keep the harness thin and the skill capabilities strong. The root skill routes
the work, asks only the necessary user-facing choices, then loads the specific
internal module needed for the chosen direction.
