---
name: slide-narrative-planner
description: >-
  Plan the story, argument, outline, slide jobs, evidence needs, and speaking
  rhythm for Hanbao single-file HTML slide decks. Use before visual generation
  when the user provides raw notes, documents, a topic, an article, a pitch,
  academic/research material, or a script-like source and the deck needs a
  clearer narrative structure. This skill distills durable planning methods
  from deck-planner, research-deck, and click-story without owning
  rendering or style.
---

# Slide Narrative Planner

This skill turns raw material into a deck plan. It does not render HTML and does
not choose the final visual path. Its output is a concise, usable plan for the
main slide generator.

Use it when the deck needs story structure, evidence hierarchy, slide-by-slide
jobs, speaker rhythm, or when the input is longer than a simple prompt.

This module keeps fuller retained material, but the Hanbao rules in this file are
the adapter. Load retained references only when the deck needs that depth.

## Output Contract

Return a compact planning artifact that the generator can use directly:

```text
Deck goal:
Audience:
Core claim:
Narrative shape:
Slide count:

Slides:
1. Action title
   Job:
   Evidence/content:
   Visual type:
   Speaker beat:
   Asset need:
...

Gaps / assumptions:
```

Do not produce a full deck. Do not make visual styling decisions beyond
high-level visual type suggestions.

## Core Rules

- One deck has one core claim or decision path.
- Each slide has one job.
- Prefer action titles: complete takeaway sentences, not topic labels.
- Reading the action titles in sequence should tell the whole story.
- Use evidence, examples, data, quotes, screenshots, diagrams, or comparisons
  instead of long paragraphs.
- If the deck is for speech, plan click beats or speaker beats separately from
  visible text.
- If the source is a long article or script, preserve the source as the detail
  pool and extract only what each slide needs.
- If evidence is missing, mark the gap instead of inventing facts.

## When To Run

Run this before `hanbao-html-slide` when:

- the user gives a topic only and needs a coherent deck,
- the user provides a long article, report, notes, or transcript,
- the deck is pitch, sales, academic, executive, research, or teaching material,
- the user asks for speaker notes or a talk flow,
- the deck needs a stronger story before visuals.

Skip it when:

- the user gives an already approved slide-by-slide outline,
- the task is only visual polish of an existing deck,
- the user asks for a tiny one-off demo with explicit direct generation.

## Planning Patterns

### Pyramid Argument

Use for business, product, sales, executive, and pitch decks.

For detailed intake, workflow, template, visualization, and scoring guidance,
read selectively from:

- `references/deck-planner/full-reference.md`
- `references/deck-planner/references/INTAKE.md`
- `references/deck-planner/references/WORKFLOW.md`
- `references/deck-planner/references/TEMPLATES.md`
- `references/deck-planner/references/VIS-GUIDE.md`
- `references/deck-planner/references/RUBRIC.md`
- `references/deck-planner/references/CHECKLIST.md`

`scripts/deck-planner/chartkit.py` is retained for chart rendering guidance when
the deck has real tabular data, but the default Hanbao output remains single-file
HTML.

1. Core claim.
2. Three to five supporting reasons.
3. Evidence pages for the reasons.
4. Risk / objection handling if relevant.
5. Close with decision or next action.

### Assertion-Evidence

Use for analytical and data-heavy decks.

- Title states the takeaway.
- Body proves the takeaway.
- Each chart or table has a "so what" annotation.
- Labels include units, dates, and source when available.

### Academic / Research

Use for papers, thesis, seminar, grant, and evidence-evaluated talks.

For full academic content rules, read selectively from:

- `references/research-deck/full-reference.md`
- `references/research-deck/content_guidelines.md`
- `references/research-deck/slide_patterns.md`

- Argument clarity outranks decoration.
- Every borrowed figure, quote, or data point needs a citation/source note.
- One exhibit per result slide when possible.
- Conclusions should be the last main slide.
- Add appendix candidates rather than overloading main slides.

### Video-Like / Click-Beat

Use when the deck is meant to feel like a recorded explainer or dynamic talk.

For full click-driven/video-like planning guidance, read selectively from:

- `references/click-story/full-reference.md`
- `references/click-story/references/SCRIPT-STYLE.md`
- `references/click-story/references/OUTLINE-FORMAT.md`
- `references/click-story/references/CHAPTER-CRAFT.md`

The original web-video Vite templates are retained under
`assets/click-story/templates/` for reference only. Do not output a
Vite/React project from Hanbao; adapt the cadence and chapter craft into a
single-file HTML deck.

- Convert paragraphs into spoken beats.
- Each click advances one narrative beat.
- Keep each screen focused on the current sentence or idea.
- Outline content density and step count, but do not pre-write exact animation.
- Leave animation choice to `slide-motion-polish` during visual generation.

## Visual Type Suggestions

Use these visual types in the plan so the generator can pick layouts:

- cover
- agenda
- thesis statement
- section divider
- big stat
- data contrast
- comparison
- process / pipeline
- timeline
- product screenshot / UI mock
- architecture / system diagram
- evidence image
- quote
- case study
- risk / mitigation
- close / ask

Decision order:

1. sequence -> process or timeline
2. two or more alternatives -> comparison
3. metric contrast -> data contrast or big stat
4. proof from source -> evidence image / chart / quote
5. product behavior -> screenshot / UI mock
6. concept structure -> architecture / diagram
7. otherwise -> thesis statement or two-column narrative

## Self-Check

Before handing the plan to the generator:

- Do the action titles alone tell a coherent story?
- Is every slide's job distinct?
- Are unsupported claims marked as assumptions or gaps?
- Is visible text likely to fit on a slide?
- Are speaker beats separated from visible copy?
- Is the plan specific enough for visual generation without rereading all source?

## Reference Routing

| Need | Read |
| --- | --- |
| Business/pitch/report structure | `references/deck-planner/references/WORKFLOW.md` |
| Minimal intake questions | `references/deck-planner/references/INTAKE.md` |
| Layout/slide type ideas | `references/deck-planner/references/TEMPLATES.md` |
| Chart/data visualization decisions | `references/deck-planner/references/VIS-GUIDE.md` |
| Quality scoring | `references/deck-planner/references/RUBRIC.md` |
| Academic argument rules | `references/research-deck/content_guidelines.md` |
| Academic slide patterns | `references/research-deck/slide_patterns.md` |
| Script/click-beat transformation | `references/click-story/references/SCRIPT-STYLE.md` and `OUTLINE-FORMAT.md` |
| Dynamic chapter craft | `references/click-story/references/CHAPTER-CRAFT.md` |

Do not read all retained references for a simple deck.
