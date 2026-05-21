---
name: hanbao-slide-skills
description: >-
  One packaged Hanbao HTML slide skill system for generating, styling, auditing,
  source-grounding, narrative-planning, visual-asset generation, and
  motion-polishing single-file HTML slide decks. Use when the
  user asks to create, improve, audit, polish, style, or optimize PPT/slides/
  decks/presentations/HTML slides for product, sales, pitch, report,
  teaching, or sharing scenarios. This package contains independent internal
  modules and keeps retained upstream resources while routing to only the
  needed branch after selection.
---

# Hanbao Slide Skills

This is a **single uploadable skill package** containing independent internal modules for the Hanbao HTML slide workflow.

Use the internal modules independently when needed, but treat this root package as the entry point when the system expects one `SKILL.md`.

See `ROUTING.md` for how user-facing choices map to retained resources without bulk-reading every upstream file.

Default behavior is **entry-choice-first**. Before doing slide generation work, ask the user to choose one of three paths: direct generation, style selection, or template/case selection. If the user's initial prompt already explicitly chooses one of these paths, follow that path.

Typography selection is mandatory and happens after the concrete style or template/case choice. Before generating the final deck, always ask the user to choose a typography preset. Do not silently default to any font preset.

## Internal Modules

### 1. `slide-source-grounding`

Source extraction and evidence grounding.

- Path: `slide-source-grounding/SKILL.md`
- Distills progressive retrieval from `kb-retriever` and document/table handling patterns.
- Output: compact evidence pack with source locations, claims, data points, quotes, and gaps.
- Use before narrative planning when the deck is based on documents, folders, PDFs, DOCX, XLSX/CSV, or knowledge-base material.
- Does not render slides or decide visual style.

### 2. `slide-narrative-planner`

Story and deck planning.

- Path: `slide-narrative-planner/SKILL.md`
- Distills `ppt-creator`, `academic-pptx`, and `web-video-presentation` planning methods.
- Output: core claim, slide jobs, action titles, evidence needs, visual type suggestions, and optional click beats.
- Use before visual generation when raw material needs structure.
- Does not render slides or own style.

### 3. `hanbao-html-slide`

Main generator.

- Path: `hanbao-html-slide/SKILL.md`
- Distills: `html-ppt`, `guizang-ppt-skill`, `frontend-slides`, and single-file-safe parts of `huashu-design`.
- Output: exactly one self-contained HTML deck.
- Supports: direct generation, user-facing style choice, and user-facing case/example choice.
- Must not create React/Vite projects, build steps, dev servers, or multi-file artifacts.
- Keeps real reusable resources: html-ppt base/runtime/layouts/themes/animations, guizang magazine/Swiss templates and references, frontend-slides viewport/style-preview references.
- For guizang magazine/editorial output, preserve the original signature WebGL dual-background effect from `assets/guizang/template.html`: `canvas#bg-dark`, `canvas#bg-light`, fluid shader motion, `startGL()`, hero low-opacity overlays, and light/dark background switching. Do not replace it with a flat color or static gradient.
- Also indexes huashu-derived single-file-safe style resources under the Warm Studio direction.

### 4. `slide-visual-assets`

Visual asset planning and image generation module.

- Path: `slide-visual-assets/SKILL.md`
- Distills image planning and generation from `gpt-image-2`, host-native image generation, `imagegen`, and guizang image guidance.
- Output: generated image paths when an image API/tool exists; otherwise no-image or HTML-visual fallback decisions. It does not output prompts as the user-facing result.
- High-priority default: when the environment supports image2 / GPT Image 2 / host-native image generation, run this module for every non-text-only deck before final HTML generation unless the user explicitly says not to use generated images.
- Use generated images for cover/chapter anchors, product scenes, evidence visuals, diagrams, UI mockups, and visually rich slides where imagery improves comprehension or polish.
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
- Distills the audit and polish side of `impeccable`.
- Checks hierarchy, density, layout rhythm, color strategy, typography, copy clarity, responsiveness, and anti-AI-template issues.
- Runs after generation by default.
- Keeps focused audit references for critique, polish, layout, typeset, responsive behavior, UX writing, color, and cognitive load.

### 7. `slide-motion-polish`

Full-power motion, interaction, and visual polish module.

- Path: `slide-motion-polish/SKILL.md`
- Preserves the `emil-design-eng` design engineering framework.
- Must be used during deck generation, not only after generation.
- Shapes page transitions, slide entry choreography, local reveal motion, layout rhythm, button/control feedback, panels, presenter controls, and reduced motion.
- Runs again after visual audit by default.
- Keeps motion and interaction references so the original optimization depth is not reduced to a short checklist.
- Also retains the full original `emil-design-eng` SKILL.md as a reference.

## Recommended Pipeline

```text
user content
  -> slide-source-grounding when source files/folders/evidence are involved
  -> slide-narrative-planner when structure/story is needed
  -> hanbao-html-slide
  -> choose route/style/case and typography
  -> create deck plan and slide jobs
  -> slide-visual-assets before final HTML writing whenever image2 or host-native image generation is available, unless the user explicitly opts out
  -> slide-motion-polish for transition/layout/motion planning
  -> slide-visual-audit
  -> slide-motion-polish
  -> browser verification
  -> final single HTML deck
```

## Parallel Execution Strategy

Keep orchestration thin but avoid unnecessary serial waiting:

- After the deck plan is locked, generate independent visual assets in parallel. Each image request should own one slide/asset and write to a unique output path.
- While visual assets are being generated, continue non-conflicting work: selected-branch template preparation, slide copy tightening, typography token mapping, and motion grammar planning.
- After the first complete HTML draft exists, run `slide-visual-audit` and `slide-motion-polish` as parallel review passes over the same artifact when the host supports parallel work. Merge their findings into one edit pass.
- Do not run parallel edits against the same HTML file. Parallel work may produce plans, assets, or findings; the final HTML integration remains a single controlled pass.
- Browser verification runs after integration, not in parallel with file mutation.

## Usage Rules

- Output deck artifacts must remain single-file HTML unless the user explicitly asks otherwise.
- Keep the harness thin: use the root package for routing, then load only the specific module needed.
- Do not read every module by default. Source grounding and narrative planning are conditional. Visual assets are high priority when image generation is available and the user has not opted out.
- First ask the user how to set direction: direct generation, choose a visual style, or choose a template/case direction. Do this before reading detailed branch resources.
- If the user chooses style selection, offer 2-3 concise visual direction options next. Do not expose internal branch names or source skill names as the user's choices.
- If the user chooses template/case selection, offer 2-3 concise template/case direction options next. Do not expose internal branch names or source skill names as the user's choices.
- Direct generation is allowed only after the user chooses direct generation or uses explicit wording such as "directly generate", "just make it", "no need to ask", "quick draft", "直接出", "不用问", "快速生成", or "一把梭".
- Ask for typography only after the route is concrete: after direct generation is chosen, after a specific style option is chosen, or after a specific template/case option is chosen.
- Before final generation, always ask the user to choose a typography preset: Hanbao Default, Editorial Luxe, Swiss Light, or Editorial Serif. `Editorial Luxe` is the premium serif option, but it is not automatic.
- After the user chooses an option, internally route to the matching branch and do not read every retained resource.
- Keep the internal modules separate so each effect can be tested independently.
- Do not collapse this package into a tiny checklist. The package intentionally retains useful upstream resources.
- Do not include a live tweak panel in the Hanbao slide package. When design variants are needed, generate alternate style/case options before production rather than adding runtime controls to the final deck.
