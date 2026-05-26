---
name: hanbao-html-slide-full
description: >-
  Full integrated single-file HTML slide skill with image generation. Use this
  skill whenever the user asks to generate, create, design, rewrite, polish,
  audit, fix, convert, or optimize slides, PPT, presentation decks, pitch decks,
  report decks, teaching decks, product demos, roadshow decks, share decks,
  HTML PPT, HTML slides, web presentations, animated 16:9 slides, data slides,
  chart slides, KPI pages, or dashboard-style slides. Also use for Chinese
  requests such as PPT, 幻灯片, 演示文稿, 汇报, 路演, 课件, 报告页, 分享页,
  网页演示, HTML幻灯片, 单文件HTML演示, 图表页, 数据看板. Produces exactly
  one self-contained HTML deck. Includes source grounding, narrative planning,
  style/template selection, typography selection, data visualization, generated
  visual assets via image generation/host image generation when available, full-power motion, visual
  audit, and browser-fit checks.
---

# Hanbao Slide Skills

This is a **single uploadable skill package** containing independent internal modules for the Hanbao HTML slide workflow.

## Skill Index

This root `SKILL.md` is the upload and retrieval entry point for the full slide package.

Trigger this skill for any request whose desired output is a slide deck, PPT-like presentation, HTML presentation, web slide, animated deck, report deck, pitch deck, teaching deck, product demo deck, data presentation, chart slide, or dashboard slide.

Use the full package when generated images are allowed or when the environment exposes image generation / host image generation. If the user explicitly asks for no generated images, use the no-image package instead.

Use the internal modules independently when needed, but treat this root package as the entry point when the system expects one `SKILL.md`.

See `ROUTING.md` for how user-facing choices map to retained resources without bulk-reading every retained file.

Default behavior is **entry-choice-first**. Before doing slide generation work, ask the user to choose one of three paths: direct generation, style selection, or template/case selection. If the user's initial prompt already explicitly chooses one of these paths, follow that path.

Typography resolution is mandatory, but direct generation should not add a font-choice step. For direct generation, the agent selects the typography preset that best fits the audience, content density, and visual direction. For style selection or template/case selection, ask the user to choose a typography preset after the concrete style or case is chosen.

Before planning or generating, convert the user's request into a production brief centered on the deck's audience: who will watch/read it, what they should understand, what decision or impression the deck should support, what information each slide must present, what evidence is needed, what visual direction fits the audience, what image/data responsibilities exist, what motion should clarify, and what output constraints apply. This brief is a production aid; user-facing replies should ask only necessary choices or report final artifact status, and the deck itself should present audience-facing content.

User-facing language should stay result-facing. In chat and visible slide
content, frame choices around what the audience will experience: visual tone,
information density, motion feel, chart treatment, typography feel, and the
decision each slide helps the viewer make. Use implementation details as working
context for building the deck, while the finished communication stays centered
on the viewer and the artifact.

## Mandatory Generation Gates

These gates are required. A final HTML deck is not valid until every applicable gate has been completed.

1. Audience brief gate: define intended viewer, viewer need, desired takeaway, evidence needs, slide jobs, data needs, image needs, motion intent, and output constraints.
2. Direction gate: ask the three-path entry question unless the user explicitly requested direct generation. If the user chooses style selection, collect that concrete choice before continuing. If the user chooses template/case selection, generate three visible one-slide template previews, one per candidate style, and collect the user's choice from those previews.
3. Typography gate: resolve a typography preset before writing final HTML. In direct generation, auto-select the preset and record why it fits. In style or template/case selection, ask the user after the concrete direction is locked.
4. Planning gate: create a deck plan with slide count, slide jobs, content hierarchy, evidence/data requirements, visual role per slide, and click/reveal rhythm. Planning is mandatory even for direct generation.
5. Data visual gate: scan the plan for numbers, comparisons, KPI cards, tables, charts, trends, timelines, dashboards, matrices, or structured evidence. If present, run `slide-data-visuals` before writing final HTML.
6. Visual asset gate: in the full package, when image generation / host image generation is available and the user has not opted out, run `slide-visual-assets` before final HTML and assign each asset a slide responsibility.
7. Motion gate: use `slide-motion-polish` before final HTML to define page transitions, slide entry choreography, local reveal motion, KPI/count-up behavior, interaction feedback, and reduced-motion behavior.
8. Quality gate: run visual audit and motion polish after the first draft, then verify normal-browser fit, keyboard navigation, direct slide links, no blank slides, and no bottom-content clipping.

## Full-Power Default

Default to the strongest available production quality, not a minimal draft.

- Visual design full-power: use a deliberate art direction, strong first screen, varied slide rhythm, polished hierarchy, premium typography, stable spacing, and non-generic composition.
- Motion full-power: page transitions, entrance choreography, staggered reveals, chart draw-ins, KPI/count-up animation, directional continuity, hover/press feedback where controls exist, and reduced-motion fallback are expected by default.
- Image full-power in the full package: when image generation is available and not explicitly disabled, generate multiple role-specific assets rather than one generic cover image.
- Data full-power: when content contains numbers, comparisons, timelines, KPIs, tables, structured claims, or dashboard-like material, use native inline visual components instead of plain bullets.
- Audit full-power: visual audit and motion polish are default gates, not optional decoration.

Only reduce power when the user explicitly asks for static, fast, low-motion, text-only, no-image, or rough-draft output.

## Internal Modules

### 1. `slide-source-grounding`

Source extraction and evidence grounding.

- Path: `slide-source-grounding/SKILL.md`
- Distills progressive retrieval from `source-reader` and document/table handling patterns.
- Output: compact evidence pack with source locations, claims, data points, quotes, and gaps.
- Use before narrative planning when the deck is based on documents, folders, PDFs, DOCX, XLSX/CSV, or knowledge-base material.
- Does not render slides or decide visual style.

### 2. `slide-narrative-planner`

Story and deck planning.

- Path: `slide-narrative-planner/SKILL.md`
- Distills `deck-planner`, `research-deck`, and `click-story` planning methods.
- Output: core claim, slide jobs, action titles, evidence needs, visual type suggestions, and optional click beats.
- Use before visual generation when raw material needs structure.
- Does not render slides or own style.

### 3. `hanbao-html-slide`

Main generator.

- Path: `hanbao-html-slide/SKILL.md`
- Distills: `studio-deck`, `Editorial Motion system`, `viewport-showcase`, and single-file-safe parts of `warm-studio`.
- Output: exactly one self-contained HTML deck.
- Supports: direct generation, user-facing style choice, and user-facing case/example choice.
- Must not create React/Vite projects, build steps, dev servers, or multi-file artifacts.
- Keeps real reusable resources: studio-deck base/runtime/layouts/themes/animations, Editorial Motion magazine/Swiss templates and references, viewport-showcase viewport/style-preview references.
- For Editorial Motion magazine/editorial output, preserve the original signature WebGL dual-background effect from `assets/editorial-motion/template.html`: `canvas#bg-dark`, `canvas#bg-light`, fluid shader motion, `startGL()`, hero low-opacity overlays, and light/dark background switching. Do not replace it with a flat color or static gradient.
- Also indexes Warm Studio single-file-safe style resources under the Warm Studio direction.

### 4. `slide-visual-assets`

Visual asset planning and image generation module.

- Path: `slide-visual-assets/SKILL.md`
- Distills image planning and generation from `raster-engine`, host image generation, `host image generation`, and editorial-motion image guidance.
- Output: generated image paths when an image API/tool exists; otherwise no-image or HTML-visual fallback decisions. It does not output prompts as the user-facing result.
- Mandatory max-power default: when the environment supports image generation / host image generation, run this module for every non-text-only deck before final HTML generation unless the user explicitly says not to use generated images.
- Use multiple generated image assets with distinct responsibilities: cover/chapter anchor, product or use-case scene, evidence/context visual, diagram/architecture visual, UI mock/state visual, and optional atmosphere/texture only when it supports the story.
- Does not own deck layout.

### 5. `slide-typography`

Typography preset module.

- Path: `slide-typography/SKILL.md`
- Controls type tokens only.
- Does not generate the whole deck.
- Includes: `Hanbao Default`, `Editorial Luxe`, `Swiss Light`, `Editorial Serif`.
- `Editorial Luxe` is the premium high-contrast serif typography preset with italic emphasis and B2B editorial polish.

### 6. `slide-visual-audit`

Default visual quality gate.

- Path: `slide-visual-audit/SKILL.md`
- Distills the audit and polish side of `visual-audit`.
- Checks hierarchy, density, layout rhythm, color strategy, typography, copy clarity, responsiveness, and anti-AI-template issues.
- Runs after generation by default.
- Keeps focused audit references for critique, polish, layout, typeset, responsive behavior, UX writing, color, and cognitive load.

### 7. `slide-motion-polish`

Full-power motion, interaction, and visual polish module.

- Path: `slide-motion-polish/SKILL.md`
- Preserves the `motion-craft` design engineering framework.
- Must be used at full power during deck generation, not only after generation.
- Shapes page transitions, slide entry choreography, local reveal motion, layout rhythm, button/control feedback, panels, presenter controls, and reduced motion.
- Runs again after visual audit by default.
- Keeps motion and interaction references so the original optimization depth is not reduced to a short checklist.
- Also retains the full original `motion-craft` SKILL.md as a reference.

### 8. `slide-data-visuals`

Chart, KPI, table, and dashboard component module.

- Path: `slide-data-visuals/SKILL.md`
- Distills Data Visuals data-analysis, infographic, and PPT slot-planning patterns for single-file HTML slides.
- Output: embeddable inline SVG/HTML/CSS/JS component specs and implementation notes, not standalone chart images or separate dashboards.
- Use whenever a deck has a genuine chart need: Excel/CSV-derived charts, trend lines, KPI cards, comparison tables, heatmaps, stacked charts, or dashboard pages.
- This is the non-image chart generation path. Do not route chart needs into image generation unless the user explicitly asks for chart screenshots or raster exports.
- Charts and dashboards must inherit the selected slide style, background, typography, tokens, and motion system.

## Recommended Pipeline

```text
user content
  -> slide-source-grounding when source files/folders/evidence are involved
  -> slide-narrative-planner when structure/story is needed
  -> hanbao-html-slide
  -> choose route/style/case and resolve typography
  -> create deck plan and slide jobs
  -> slide-data-visuals before final HTML writing when slides need charts, KPIs, tables, or dashboards
  -> slide-visual-assets before final HTML writing whenever image generation or host image generation is available, unless the user explicitly opts out
  -> slide-motion-polish for transition/layout/motion planning
  -> slide-visual-audit
  -> slide-motion-polish
  -> browser verification
  -> final single HTML deck
```

## Parallel Execution Strategy

Keep orchestration thin but avoid unnecessary serial waiting:

- After the deck plan is locked, generate independent visual assets and data-visual specs in parallel. Each image request should own one slide/asset and a clear responsibility; each data-visual request should own one chart/dashboard/table spec.
- While visual assets or data-visual specs are being produced, continue non-conflicting work: selected-path template preparation, slide copy tightening, typography token mapping, and motion grammar planning.
- After the first complete HTML draft exists, run `slide-visual-audit` and `slide-motion-polish` as parallel review passes over the same artifact when the host supports parallel work. Merge their findings into one edit pass.
- Do not run parallel edits against the same HTML file. Parallel work may produce plans, assets, or findings; the final HTML integration remains a single controlled pass.
- Browser verification runs after integration, not in parallel with file mutation.

## Usage Rules

- Output deck artifacts must remain single-file HTML unless the user explicitly asks otherwise.
- Keep the harness thin: use the root package for routing, then load only the specific module needed.
- Do not read every module by default. Source grounding and narrative planning are conditional. Data visuals are used only for chart/KPI/table/dashboard needs. Visual assets are mandatory at max power when image generation / host image generation is available and the user has not opted out.
- The mandatory gates above override shortcuts. If Hanbao has not collected the required user choice, plan, typography resolution, data-visual decision, motion plan, and audit result, continue that gate instead of producing final HTML.
- First ask the user how to set direction: direct generation, choose a visual style, or choose a template/case direction. Do this before reading detailed path resources.
- If the user chooses style selection, offer 2-3 concise visual direction options next. Phrase each option by audience fit, visual tone, information density, and presentation effect.
- If the user chooses template/case selection, generate three visible one-slide HTML template previews next. Each preview must be an actual single-slide visual sample for a distinct candidate style, not a text-only description. Phrase the choice by what the viewer can see in the preview, then wait for the user to choose one.
- Direct generation is allowed only after the user chooses direct generation or uses explicit wording such as "directly generate", "just make it", "no need to ask", "quick draft", "直接出", "不用问", "快速生成", or "一把梭".
- Direct generation skips the font-choice question. Auto-select one typography preset internally: Hanbao Default, Editorial Luxe, Swiss Light, or Editorial Serif.
- Ask the user for typography only after a specific style option or a specific rendered template/case preview is chosen. `Editorial Luxe` is the premium serif option, but it is not automatic.
- After the user chooses an option, internally route to the matching path and do not read every retained resource.
- Keep the internal modules separate so each effect can be tested independently.
- Do not collapse this package into a tiny checklist. The package intentionally retains useful retained resources.
- Do not include a live tweak panel in the Hanbao slide package. When design variants are needed, generate alternate style/case options before production rather than adding runtime controls to the final deck.
- Make the final deck answer the viewer's questions: who this is for, what the message is, why it matters, what evidence supports it, and what the viewer should remember or do next.
- When replying in chat during generation, lead with the next user choice, concise progress, file path, verification status, or genuinely needed clarification. Make the wording about the deck's intended viewer and finished effect, so the conversation feels like production guidance rather than an implementation log.
