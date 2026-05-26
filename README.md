# Hanbao Slide Skills

Hanbao Slide Skills is a single uploadable skill package for producing polished,
single-file HTML slide decks. It combines several proven slide/design skill
patterns, but reorganizes them into a thinner routing layer plus independent
specialist modules.

## What It Does

- Generates self-contained HTML slide decks.
- Supports direct generation, style selection, and template/case selection.
- In template/case mode, generates three one-slide visual previews in different
  styles so the user chooses by seeing the result, not by reading descriptions.
- Auto-selects typography for direct generation; asks for typography after a
  concrete style or rendered template preview is chosen.
- Uses source grounding and narrative planning when the input needs them.
- Uses inline data visuals for chart, KPI, table, matrix, and dashboard-style
  slides.
- Uses generated visual assets when image generation is available and the user
  has not opted out.
- Runs motion polish and visual audit as default quality gates.

## Open Skills And Patterns Referenced

This package borrows capability patterns from these open/local skills and
adapts them for one-file HTML slide generation:

| Source skill / pattern | What was useful | How it is adapted here |
| --- | --- | --- |
| `html-ppt` / studio deck patterns | Stable HTML deck runtime, slide layouts, themes, navigation, presenter concepts | Renamed internally as `studio-deck`; used for reliable product/report/pitch skeletons |
| `guizang-ppt-skill` style patterns | Magazine/editorial layouts, Swiss grid discipline, WebGL dual-background hero effect | Renamed internally as `editorial-motion`; used for premium editorial and Swiss-style decks |
| `frontend-slides` patterns | Animation-rich frontend slide craft, viewport fitting, style-preview thinking | Renamed internally as `viewport-showcase`; used for strict browser-fit and preview-oriented generation |
| `huashu-design` patterns | Comfortable typography, high-fidelity HTML composition, refined product/demo page feel | Renamed internally as `warm-studio`; used as a single-file-safe visual style resource |
| `ppt-creator` / deck-planning patterns | Slide jobs, action titles, evidence hierarchy, story structure, chart planning | Renamed internally as `deck-planner`; used inside `slide-narrative-planner` |
| `academic-pptx` patterns | Research/lecture structure, evidence hierarchy, academic slide conventions | Renamed internally as `research-deck`; used when content is technical or research-heavy |
| `web-video-presentation` patterns | Click-beat pacing, chapter rhythm, video-like sequencing | Renamed internally as `click-story`; used for dynamic narrative decks |
| source retrieval / document-grounding patterns | Extracting claims, quotes, tables, gaps, and source locations | Renamed internally as `source-reader`; used inside `slide-source-grounding` |
| image generation patterns | Asset role planning, prompt strengthening, local/host image generation | Renamed internally as `raster-engine`; used inside `slide-visual-assets` |
| visual audit patterns | Hierarchy, density, responsive fit, typography, color, anti-template checks | Used inside `slide-visual-audit` as a default post-generation quality gate |
| motion design patterns | Page transitions, entry choreography, numeric motion, chart draw-ins, interaction feedback | Renamed internally as `motion-craft`; used inside `slide-motion-polish` |

The published package avoids exposing most upstream names in runtime routing.
The names above are documented here for attribution and review clarity.

## What Is New In This Package

This is not just a bundle of copied skills. The package adds a new integrated
architecture around them:

1. Single uploadable root, modular internals

   The root `SKILL.md` is the only required entry point. It routes work into
   independent modules such as source grounding, narrative planning, HTML slide
   generation, visual assets, typography, visual audit, motion polish, and data
   visuals. Each module can still be inspected and tested separately.

2. Entry-choice-first workflow

   The skill first decides whether the user wants direct generation, style
   selection, or template/case selection. This keeps the interaction clear
   without forcing every user through the same long intake.

3. Rendered template preview selection

   Template/case selection must generate three actual one-slide HTML previews in
   different styles. The user chooses based on visible design results, not text
   descriptions.

4. Direct generation skips font choice

   If the user asks to generate directly, the agent auto-selects typography.
   If the user chooses a style or rendered template preview, typography is asked
   afterward as the final pre-generation choice.

5. Full-power image path

   When image generation is available, images are not treated as optional cover
   decoration. The visual asset module plans multiple role-specific assets:
   cover/chapter anchor, product/use-case scene, evidence/context visual,
   diagram/architecture visual, UI/state visual, and supporting texture only
   when useful.

6. Non-image data visual path

   Chart, KPI, table, matrix, and dashboard needs route through
   `slide-data-visuals`. These outputs are inline HTML/SVG/CSS/JS components
   that inherit the selected slide style instead of becoming disconnected chart
   screenshots.

7. Motion as a generation-time design layer

   Motion is planned before HTML is written, not added as a decorative cleanup.
   The package emphasizes page transitions, entry choreography, numeric motion,
   chart draw-ins, interaction states, and reduced-motion behavior.

8. Default audit and fit gates

   The generated deck must pass visual audit, motion polish, browser-fit checks,
   keyboard navigation, direct slide linking, no blank slides, and no bottom
   clipping at normal browser zoom.

## Architecture

```text
SKILL.md
ROUTING.md

hanbao-html-slide/
  Main single-file HTML deck generator

slide-source-grounding/
  Evidence extraction from files, folders, docs, PDFs, sheets, and notes

slide-narrative-planner/
  Story structure, slide jobs, click beats, action titles, evidence planning

slide-data-visuals/
  Inline chart, KPI, table, matrix, and dashboard components

slide-visual-assets/
  Image role planning and environment-provided image generation

slide-typography/
  Typography presets and type-token guidance

slide-visual-audit/
  Layout, hierarchy, responsive fit, copy, color, and polish checks

slide-motion-polish/
  Page transitions, local reveals, numeric motion, interaction feedback
```

## Design Philosophy

The package follows a "thin harness, strong skills" philosophy. The root skill
does only routing, choice handling, and quality gates. Durable capability lives
in the modules and references, and the agent loads only the branch needed for
the user's selected direction.
