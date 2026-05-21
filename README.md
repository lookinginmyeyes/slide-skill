# Hanbao Slide Skills

Hanbao Slide Skills is a single uploadable skill package for generating polished, single-file HTML slide decks. It keeps the modules separate so each capability can be tested independently while still working as one package.

## What It Does

- Generates self-contained HTML slide decks.
- Supports direct generation, style selection, and template/case selection.
- Requires typography selection before final generation.
- Uses source grounding and narrative planning when the input needs them.
- Uses generated visual assets when an image generation capability is available and the user has not opted out.
- Runs visual audit and motion polish as quality gates.

## Referenced And Distilled Skills

This package integrates and adapts useful parts from these skills and resources:

- `html-ppt`: base HTML deck runtime, layouts, themes, presenter concepts, and animation options.
- `guizang-ppt-skill`: magazine/editorial and Swiss-style HTML slide systems, including the signature WebGL background behavior for the magazine branch.
- `frontend-slides`: viewport handling, style-preview thinking, animation-rich HTML presentation patterns, and density constraints.
- `huashu-design`: single-file-safe high-fidelity HTML design patterns, typography feel, visual anchors, and review discipline.
- `ppt-creator`: narrative planning, slide jobs, action titles, evidence hierarchy, and data/chart planning ideas.
- `academic-pptx`: academic content structure, evidence hierarchy, and lecture/research slide patterns.
- `web-video-presentation`: click-beat planning, video-like pacing, chapter craft, and narration-aware sequencing.
- `kb-retriever`: progressive source retrieval and document/table grounding patterns.
- `gpt-image-2` / `image2`: visual asset planning, image generation, and local image generation/editing scripts.
- `impeccable`: visual audit, layout critique, hierarchy, density, typography, and polish checks.
- `emil-design-eng`: motion, interaction, page transitions, timing, easing, feedback, and invisible UI polish.

## Design Principle

The package keeps the harness thin and the skills strong. The root skill handles routing and user-facing choices; the internal modules carry the durable behavior. The agent should load only the module and references needed for the selected direction instead of reading every retained resource.

## Package Structure

```text
SKILL.md
ROUTING.md
hanbao-html-slide/
slide-source-grounding/
slide-narrative-planner/
slide-visual-assets/
slide-typography/
slide-visual-audit/
slide-motion-polish/
```
