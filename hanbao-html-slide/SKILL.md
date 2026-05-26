---
name: hanbao-html-slide
description: >-
  Generate a polished single-file HTML slide deck for Hanbao-style product, sales,
  pitch, report, teaching, or sharing presentations. Use when the user asks to
  create PPT, slides, deck, presentation, keynote, HTML deck, web PPT, HTML
  slides, or wants style selection or case selection before generation. This
  skill distills studio-deck, Editorial Motion system, viewport-showcase, and the
  single-file-safe parts of warm-studio into one main generator. Output must
  always be one self-contained HTML file.
---

# Hanbao HTML Slide

Main generation skill for Hanbao-compatible HTML slide decks.

This skill generates **one self-contained HTML file**. It may use uploaded or system asset URLs, but it must not create a React/Vite project, require a dev server, or output a multi-file artifact.

Important: do not reduce or remove useful retained capabilities. This is an integration and routing skill, not a minimal rewrite. Keep the useful template/runtime/layout/theme/preview capabilities available, but load only the path needed for the user's chosen style path.

Use the `slide-motion-polish` / `motion-craft` framework at full power during deck generation. It is not just a final cleanup pass. The deck skeleton, page transitions, slide entry choreography, local reveal effects, press/hover feedback, panel behavior, and visual rhythm should be designed with that framework before the HTML is written.

Keep preprocessing modular. If the input needs source extraction, story planning,
data visualization, or image generation, call the matching independent module
instead of expanding this generator into a large all-purpose harness.

Before any planning, translate the user's request into an audience-centered
production brief. Read `references/query-rewrite.md` when the request is
ambiguous, broad, or easy to answer with a generic deck. The brief should clarify:
who will watch/read the deck, what they need to understand, what decision or
impression the deck should support, what each slide must present, what evidence
is needed, and how visual/motion choices help the audience. User-facing replies
should contain choices, artifact paths, verification status, or necessary
clarifications; the deck should contain the audience-facing story.

## Hard Output Contract

- Output exactly one `.html` file as the deck artifact.
- Inline all CSS and JavaScript.
- Do not create a project directory, build step, npm dependency, Vite app, React app, or dev server requirement.
- Do not hotlink arbitrary external assets. Use system asset URLs, user-provided assets, or base64 only when appropriate.
- Use keyboard navigation: `ArrowLeft`, `ArrowRight`, `Space`, `Home`, `End`.
- Support direct linking by `?slide=N` and/or `#slide-N`.
- Every slide must fit the viewport. No accidental page scroll in desktop presentation mode.
- Every slide must be fully visible at normal browser zoom. The user must not need to zoom out or shrink the browser to see the bottom of a slide.
- Include low-height viewport handling, especially for 1280x720 and laptop browser chrome. Treat `@media (max-height: 820px)` as a required compact mode unless the selected template already proves equivalent behavior.
- Include `prefers-reduced-motion` handling.
- If speaker notes are requested, keep notes hidden from audience view.

## What This Distills

Do not "simplify away" the retained strengths:

- Keep studio-deck's runtime, notes, presenter concepts, layouts, themes, and animation options.
- Keep editorial-motion's Swiss and magazine visual systems, template discipline, and image-evidence rules.
- Keep Editorial Motion magazine's signature WebGL dual-background hero effect. When routing to Editorial Motion magazine/editorial, use `assets/editorial-motion/template.html` as the skeleton and preserve `canvas#bg-dark`, `canvas#bg-light`, shader code, `startGL()`, hero low-opacity overlays, and light/dark background switching.
- Keep viewport-showcase' style preview, viewport fitting, density limits, and animation richness.
- Keep the single-file-safe parts of Warm Studio high-fidelity HTML, especially visual anchors, typography feel, and anti-generic-design discipline.
- Keep motion-craft's full design engineering force for art direction, layout refinement, motion purpose, easing, transition feel, and invisible interaction details.

The integration layer decides **which path to use**, not which features to delete.

## Companion Modules

Use these modules as needed before or around generation:

| Situation | Module | What it returns |
| --- | --- | --- |
| User provides local files, reports, docs, PDFs, spreadsheets, or wants citations/evidence | `slide-source-grounding` | evidence pack with source locations |
| User provides messy notes/topic/article/script or needs stronger logic | `slide-narrative-planner` | deck plan, action titles, slide jobs, click beats |
| Slides need charts, KPI cards, comparison tables, heatmaps, stacked charts, or dashboard pages | `slide-data-visuals` | non-image embeddable inline SVG/HTML/CSS/JS chart and dashboard specs that inherit selected style tokens |
| Image2 / Raster Engine / host image generation is available and the user has not opted out | `slide-visual-assets` | multiple generated image paths with distinct slide responsibilities, placement notes, and fallback decisions before final HTML writing |

Do not load these modules for every run. Load the module only when the user's
input or selected direction needs that capability.

### From Data Visuals

Use for data-heavy slide components, not as a separate visual style:

- chart type selection after data cleaning and aggregation
- KPI/dashboard slot planning
- pivot, cross-tab, time-series, group-by, and comparison thinking
- infographic clarity rules for axes, labels, series distinction, and layout balance
- chart component quality gates before final HTML integration

The adapted module is `slide-data-visuals`. It must output native single-file
HTML slide components: inline SVG, semantic HTML/CSS, and small vanilla JS.
Charts and dashboards must inherit the chosen deck path's background,
typography, palette, surface treatment, radius, and motion grammar. Do not
paste standalone chart images, introduce a disconnected white dashboard theme,
or require external chart assets unless the user explicitly asks for that.
When a slide genuinely needs a chart, KPI system, comparison table, heatmap, or
dashboard, use this module as the default non-image chart generation path.

### From studio-deck

Use as the stable backbone:

- token-based theme system
- reusable layout registry
- keyboard runtime
- overview / progress / presenter notes concepts
- audience-facing slide content separated from speaker-only notes

Bundled resources kept from studio-deck:

- `assets/studio-deck/base.css`
- `assets/studio-deck/runtime.js`
- `assets/studio-deck/fonts.css`
- `assets/studio-deck/animations/`
- `assets/studio-deck/single-page/`
- selected theme CSS files in `assets/studio-deck/`
- `references/studio-deck/themes.md`
- `references/studio-deck/layouts.md`
- `references/studio-deck/animations.md`
- `references/studio-deck/presenter-mode.md`
- `references/studio-deck/authoring-guide.md`

### From Editorial Motion system

Use as premium visual modes:

- Swiss grid mode
- editorial magazine mode
- WebGL dual-background fluid hero motion on magazine covers and chapter openers
- strong chapter openers
- big stat pages
- image-as-evidence blocks
- strict layout discipline instead of freeform decoration

Bundled resources kept from editorial-motion:

- `assets/editorial-motion/template.html`
- `assets/editorial-motion/template-swiss.html`
- `assets/editorial-motion/motion.min.js`
- `references/editorial-motion/layouts.md`
- `references/editorial-motion/layouts-swiss.md`
- `references/editorial-motion/themes.md`
- `references/editorial-motion/themes-swiss.md`
- `references/editorial-motion/swiss-layout-lock.md`
- `references/editorial-motion/image-prompts.md`
- `references/editorial-motion/checklist.md`

### From viewport-showcase

Use as the selection layer:

- "show, don't tell" style previews
- three distinct visual options when style is unclear
- viewport fitting rules
- density limits per slide
- zero-dependency single HTML delivery

Bundled resources kept from viewport-showcase:

- `references/viewport-showcase/viewport-base.css`
- `references/viewport-showcase/STYLE_PRESETS.md`
- `references/viewport-showcase/html-template.md`
- `references/viewport-showcase/animation-patterns.md`
- `references/viewport-fit.md` for Hanbao-specific 100% browser zoom and low-height viewport fitting

### From warm-studio

Use only single-file-safe ideas:

- strong visual anchor before full production
- tasteful high-fidelity HTML feel
- CSS-only / vanilla JS motion
- anti-generic-design discipline

Bundled resources kept from warm-studio for single-file-safe high-fidelity direction:

- `references/warm-studio/full-reference.md`
- `references/warm-studio/design-styles.md`
- `references/warm-studio/workflow.md`
- `references/warm-studio/slide-decks.md`
- `references/warm-studio/critique-guide.md`
- `references/warm-studio/animation-best-practices.md`
- `references/warm-studio/animation-pitfalls.md`
- `references/warm-studio/verification.md`
- selected single-HTML demos in `references/warm-studio/`
- `assets/warm-studio/deck_index.html`
- `assets/warm-studio/deck_stage.js`

Do not inherit warm-studio multi-file workflows, React projects, video export pipelines, dev-server flows, or live tweak systems. If a retained retained warm-studio reference mentions Tweaks, ignore that section for Hanbao slide generation.

## Entry Choice Gate

Default behavior is **entry-choice-first**.

Before doing slide generation work or reading path-specific resources, ask the user to choose one path:

1. Direct generation
2. Choose a style
3. Choose a template/case

Ask this even for ambiguous or topic-only requests. If the user's initial prompt already explicitly chooses one of these paths, follow it without asking again.

Direct generation is allowed only when the user chooses direct generation or clearly says things like "directly generate", "just make it", "no need to ask", "quick draft", "直接出", "不用问", "快速生成", or "一把梭". Even then, still create the audience-centered brief and quality bar before generation.

Typography resolution is mandatory before final generation. Direct generation skips the font-choice question: choose the typography preset internally based on audience, content density, visual tone, and readability. Style selection and template/case selection still ask the user for typography after the concrete option is chosen.

When offering styles or cases, describe the visible direction in plain language: audience fit, visual tone, information density, motion feel, and best-use scenario. The user is choosing the result they want to show, not an implementation label.

The first question should be short, precise, and should not include implementation details. Example:

```text
这份 slide 你想怎么定方向？
1. 直接生成：我根据内容自动定风格、模板和动效
2. 先选风格：先看 2-3 个视觉方向，再生成
3. 先选模板/案例：先生成 2-3 张单页模板预览，看效果后再生成
```

## Hard Generation Gates

The generator must not write the final HTML until these gates are complete:

1. Audience-centered brief exists.
2. Entry direction is resolved: direct generation, style selection, or template/case selection.
3. If style or template/case was selected, the user has chosen one concrete option.
4. A typography preset is resolved: auto-selected for direct generation, user-selected for style or template/case paths.
5. A deck plan exists with slide count, slide jobs, hierarchy, evidence/data needs, visual role per slide, and click/reveal rhythm.
6. Every slide job has been scanned for chart/KPI/table/dashboard needs; `slide-data-visuals` has run for any matching need.
7. In the full package, image assets have been generated or intentionally skipped according to environment/user constraints.
8. A motion plan exists before HTML writing: page transition, slide entry choreography, local reveal effects, chart draw-ins, count-up/digit behavior, interaction feedback, and reduced-motion handling.
9. Post-draft visual audit, motion polish, browser-fit verification, keyboard navigation check, and direct-link check are complete.

If any gate is missing, complete that gate first. Do not compensate by producing a simpler deck.

## Full-Power Effect Standard

Default output should feel like a finished designed presentation, not a plain HTML export.

- Layout: every slide gets a clear visual role, hierarchy, rhythm, and stable responsive constraints.
- Typography: use the selected preset with intentional scale, contrast, alignment, and text fitting.
- Motion: use page transitions, per-slide entry choreography, stagger/cascade reveals, directional continuity, chart draw-ins, count-up/digit motion, and meaningful component feedback.
- Visuals: in the full package, generated assets should cover different jobs such as cover/chapter anchor, product/use-case scene, evidence/context, diagram/architecture, UI/state, or texture only when useful.
- Data: numbers and structured comparisons should become charts, KPI cards, heatmaps, timelines, matrices, or dashboard panels when that improves comprehension.
- Polish: run visual audit and motion polish as full-strength quality passes.

If the user says "quick", "draft", "static", "low motion", "text only", or "no images", obey that constraint but still keep hierarchy, fit, and clarity high.

## Path Rules

Before loading path resources, classify the request:

| User wording | Action |
| --- | --- |
| Explicit direct-generation wording | Generate immediately with a selected style path |
| Explicitly asks to choose style, or chooses entry path 2 | Offer 2-3 style options |
| Explicitly asks to choose examples/cases/templates, or chooses entry path 3 | Generate 2-3 rendered one-slide HTML template previews, then ask the user to choose |
| Ambiguous or topic-only request | Ask the three-path entry question first |

When offering options:

- Keep options short enough for the user to choose quickly.
- Use result-facing option names based on what the viewer will see.
- Describe only visible output: visual tone, first-screen feel, content-page structure, motion feel, and best-use scenario.
- Each option should include one tradeoff or best-fit note so the user can make a real choice.
- Do not read detailed path assets yet. Use only the high-level routing guidance and the prompt.
- Wait for the user's choice before loading selected path resources.

Style option format:

```text
选一个视觉方向：
1. [Name]：一句话描述画面气质。适合：...
2. [Name]：一句话描述画面气质。适合：...
3. [Name]：一句话描述画面气质。适合：...
```

Template/case preview format:

```text
选一个模板/案例方向：
我已经生成了单页模板预览：[preview-file.html]
1. [Name]：预览第 1 页，画面结构...，适合：...
2. [Name]：预览第 2 页，画面结构...，适合：...
3. [Name]：预览第 3 页，画面结构...，适合：...
```

## Typography Resolution

Before final generation, resolve one typography preset. The behavior depends on the route:

- Direct generation path: skip the typography question. Auto-select the best-fitting preset and continue.
- Style selection path: first offer 2-3 style options, wait for the user to pick one, then ask typography.
- Template/case path: first generate 2-3 rendered one-slide template/case previews, wait for the user to pick one, then ask typography.

1. Hanbao Default: clean sans typography for product, sales, and internal decks
2. Editorial Luxe: high-contrast serif headlines with italic emphasis for premium B2B storytelling
3. Swiss Light: light-weight sans typography for data-heavy, executive, and technical decks
4. Editorial Serif: quieter serif display for narrative, brand, and magazine-like decks

Rules:

- Do not leave typography unresolved.
- In direct generation, choose the typography preset internally and keep the explanation concise if mentioned.
- Do not treat Editorial Luxe as a default. It is only one option.
- If the user already explicitly selected a typography preset in the prompt, honor it and do not ask again.
- If a style or case path was chosen but the concrete style/case option has not been selected yet, do not ask typography yet.
- If a style or case has already been chosen, present the typography choice as the final pre-generation choice.
- Do not read `slide-typography/references/` just to show these options. Apply selected preset tokens directly unless custom typography or audit repair is needed.

## User Choice Modes

These are selection paths, not separate generators.

### 1. Direct Generate

Use only when the user explicitly asks for direct output.

Process:

1. Infer audience, deck type, length, and tone from the prompt.
2. Pick one style strategy based on the audience-centered brief.
3. Auto-select typography unless the user already selected one.
4. Pick a transition system and motion grammar using the full `slide-motion-polish` / `motion-craft` framework. Motion is max-power by default unless the user asked for static/low-motion output.
5. Route to exactly one primary path from the routing table below.
6. Load only that path's necessary templates/references.
7. Generate the full single-file HTML deck.
8. Run the default audit/polish chain if available.

### 2. Style Selection

Use when the user asks to pick a style or chooses entry path 2. If the prompt is ambiguous, ask the three-path entry question first instead of jumping here.

Offer 2-3 style strategies selected for the user's actual content and audience. Do not use a fixed menu every time. Each option should explain the viewer experience and the kind of deck it will produce.

Possible strategy pool:

- Clean Product: restrained product UI, clear hierarchy, quiet polish.
- Swiss Grid: strict grid, large type, data/evidence blocks.
- Editorial Magazine: serif or humanist display, image-led story, chapter rhythm.
- Warm Studio: high-fidelity product/demo feel, refined editorial-product typography, and single-file product storytelling.
- Technical Blueprint: structured engineering/report feel.
- Dark Product: focused screen/demo feel for technical or AI workflows.
- Notebook / Casebook: tactile example-led teaching or discovery.
- Investor Brief: crisp claims, metrics, ask, and narrative flow.

The selected style applies to the entire main generator. It does not mean "call only editorial-motion" or "call only viewport-showcase".

After the user selects a style, use the matching implementation path and read only the resources needed for that chosen result.

### 3. Case Selection

Use when the user wants examples or does not know the desired style.

Generate 2-3 concrete mini previews as actual one-slide HTML pages. Do not only describe them in chat. The user should be able to view the visual result and choose from what they see.

Each preview must include:

- title slide look
- content-region structure
- image/chart/data placeholder treatment when relevant
- motion feel
- typography feel
- best-use note

Preview output rules:

- Prefer one combined preview HTML file containing 2-3 single-slide preview pages with clear option labels.
- Separate one-slide HTML files are acceptable when easier to inspect.
- The preview is not the final deck. Use representative subject-aware placeholder copy, but do not invent final claims, metrics, customer names, or evidence.
- Each preview should use a different structural template/case, not just recolored versions of the same layout.
- The preview should be visually honest: if a case relies on editorial WebGL, strong charts, image-led composition, or product-demo UI framing, show that behavior in the preview page.
- After generating the preview file, give the file path and ask the user which preview to use.

After selection, lock the visual direction and generate one complete single-file HTML deck.

Case selection is also only a selection layer. It must be able to propose cases from all retained style families, not only viewport-showcase presets. Once the case is selected, continue with the implementation path that best produces that viewer-facing case.

Case candidates may come from:

- Studio Deck templates/themes for stable product, report, pitch, speaker-note decks.
- Editorial Motion Swiss or magazine templates for premium visual decks.
- viewport-showcase presets for visual discovery and strict viewport-fit examples.
- Warm Studio / Warm Studio single-file demos for refined editorial-product typography and high-fidelity product/demo pages.

For case selection:

1. Infer 2-3 distinct cases that fit the user's content.
2. Render one single-slide preview for each case.
3. Give the preview file path and a short visible-result description for each preview.
4. Wait for the user to choose one preview.
5. After selection, read only the resources needed for that chosen case.

## Style Path Routing

Pick one primary implementation path after direct/style/case selection. Load only that path's resources first. Load another path only if a slide genuinely needs a capability that the primary path lacks.

| User choice / deck need | Primary style path | Read these resources first | Do not read by default |
| --- | --- | --- | --- |
| Standard product/report/pitch deck | studio-deck | `assets/studio-deck/base.css`, `assets/studio-deck/runtime.js`, `assets/studio-deck/single-page/`, `references/studio-deck/layouts.md`, one selected theme CSS | editorial-motion templates, all frontend style presets |
| Speaker notes / presenter mode | Studio Deck presenter | `references/studio-deck/presenter-mode.md`, `assets/studio-deck/runtime.js`, relevant layouts | editorial-motion templates unless premium visual mode is selected |
| Swiss grid / executive / data-driven | Editorial Motion Swiss | `assets/editorial-motion/template-swiss.html`, `references/editorial-motion/layouts-swiss.md`, `references/editorial-motion/themes-swiss.md`, `references/editorial-motion/swiss-layout-lock.md` | studio-deck theme catalog, frontend preview docs |
| Editorial / magazine / launch story | Editorial Motion magazine | `assets/editorial-motion/template.html`, `references/editorial-motion/layouts.md`, `references/editorial-motion/themes.md`, `references/editorial-motion/image-prompts.md` | Swiss lock unless user chose Swiss; do not use studio-deck skeleton for this path |
| User wants 2-3 visual examples first | case preview router | select 2-3 candidates across studio-deck / editorial-motion / frontend / Warm Studio, then read only the chosen path | all unchosen path catalogs |
| Strict viewport-fit single HTML | frontend constraints | `references/viewport-showcase/viewport-base.css`, `references/viewport-showcase/html-template.md` | full theme catalogs |
| Dense slide / low-height browser fit | Hanbao viewport fit | `references/viewport-fit.md`, then selected path layout rules | unrelated path catalogs |
| Data-heavy chart / KPI / dashboard slide | selected path + data visuals | `slide-data-visuals/SKILL.md`, then only the relevant data visual references | standalone dashboard apps, external chart images, unrelated visual paths |
| Premium B2B editorial typography | Warm Studio or selected path + Editorial Luxe | `references/warm-studio/design-styles.md`, `references/warm-studio/workflow.md`, `assets/warm-studio/deck_index.html`, plus selected typography tokens | unrelated path catalogs |

Routing rules:

- One path owns the deck skeleton.
- If Editorial Motion magazine owns the deck skeleton, preserve the original dual WebGL background system and hero overlay behavior from `assets/editorial-motion/template.html`; do not flatten it into a static background.
- Other paths may contribute a specific feature, such as viewport rules or image asset guidance, but must not cause a full cross-path rewrite.
- The motion system is always designed deliberately, even when a path supplies a default runtime.
- Do not mix Editorial Motion Swiss and Editorial Motion magazine class systems in the same deck.
- If using Studio Deck runtime with editorial-motion-inspired visuals, keep studio-deck as runtime owner and import only the visual rules needed.
- If using editorial-motion template as skeleton, do not replace its navigation with Studio Deck runtime unless explicitly requested.
- If using Warm Studio / Warm Studio direction, preserve the single-file HTML output contract and do not import React/Vite/video-export workflows.
- If using `slide-data-visuals`, the selected deck path still owns the page skeleton. Data visuals only supply chart, table, KPI, and dashboard components that inherit the active path's tokens and background.

## Motion And Art Direction

Use `slide-motion-polish` as a generation-time design partner, not just a reviewer. Motion and interaction polish are max-power defaults, not optional decoration, unless the user asks for a static/low-motion deck.

For every deck, define:

- page transition: the global slide-to-slide movement, duration, easing, and spatial direction
- entry choreography: how title, body, image, chart, and accent elements arrive
- local reveal language: how step content, comparisons, numbers, diagrams, and highlights change
- kinetic type and numeric motion: how headlines, key terms, KPIs, percentages, dates, or rankings animate when they are the slide's focal point
- component interaction system: active/hover/focus/selected/disabled states for buttons, controls, notes, thumbnails, hotspots, tabs, toggles, sliders, legends, and popovers
- visual rhythm: how motion supports the page composition rather than masking weak layout

Rules:

- Page transitions should feel premium but remain responsive: usually 180-360ms.
- Avoid repeating the same fade-up on every slide. Vary motion by slide role and content relationship.
- Use transform and opacity first. Avoid layout-shifting animations.
- Use strong custom easing curves from the motion polish module.
- Keep keyboard navigation immediate and interruptible-feeling. Rapid key presses must not feel blocked.
- Design for reduced motion from the start.
- Do not add motion that compensates for weak hierarchy. Fix the layout first.
- When a slide is mostly visual or product-demo oriented, use richer but meaningful motion: masked reveals, parallax within bounds, chart draw-ins, pointer trails, or spotlighting.
- When a slide is driven by a key headline or number, consider kinetic typography, count-up, digit-roll, or number-morph effects during the transition or reveal.
- For hero stat, KPI, percentage, price, ranking, date, version, or before/after metric slides, make page-transition numeric motion a high-priority design option.
- For chart and dashboard slides, coordinate with `slide-data-visuals`: chart draw-ins, line traces, KPI count-ups, heatmap fades, and legend reveals must clarify reading order and stay inside stable containers.
- When a slide is dense, executive, or data-heavy, motion should be restrained and clarify reading order.
- Use tabular numbers and stable text containers so animated digits or headline motion do not cause layout jitter.
- For interactive decks, design component states before coding: hover, active, focus-visible, selected/current, disabled, and loading where relevant.
- Do not add fake interactive controls. Every control should either perform an action or be rendered as non-interactive content.

## Audience Contract

- Every slide should be written for the intended viewer, not for the maker of
  the deck. Make clear who the deck is speaking to, what they need to understand,
  what evidence supports the point, and what they should remember or do next.
- Final visible slide content should be the user's actual subject matter:
  product, strategy, report, lesson, pitch, data story, or demo. Chat replies
  should be concise: user choices, artifact path, verification status, and
  genuinely needed clarification.
- Use concrete slide jobs such as: "set context for buyers", "prove the market
  problem", "show product workflow", "compare alternatives", "summarize metrics",
  "make the recommendation", or "close with next step".
- If content is missing, use speaker-note assumptions or ask/flag the gap. The
  visible slide should still read like a finished presentation for its audience,
  not like notes about how the presentation was produced.
- Do not use placeholder claims, fake metrics, generic examples, or invented
  customer/product details to make the deck look complete.

## Layout Registry

When implementing a deck, prefer copying from bundled layout sources before inventing markup:

1. For standard template-driven decks, inspect `assets/studio-deck/single-page/` and `references/studio-deck/layouts.md`.
2. For Swiss premium pages, inspect `assets/editorial-motion/template-swiss.html` and `references/editorial-motion/layouts-swiss.md`.
3. For editorial premium pages, inspect `assets/editorial-motion/template.html` and `references/editorial-motion/layouts.md`.
4. For strict viewport fitting, apply the rules from `references/viewport-showcase/viewport-base.css`.

Only inspect the sources needed for the selected path.

Prefer these controlled layout types:

- cover
- agenda
- section divider
- thesis statement
- two-column narrative
- product UI mock
- big stat
- metric grid
- timeline
- comparison
- chart / data visual
- dashboard
- process / pipeline
- image hero
- image evidence grid
- quote
- closing

Do not invent many one-off layouts unless the content shape truly requires it.

## Viewport-Fit Contract

Hanbao decks are viewed inside browsers, product previews, and embedded surfaces.
Design for the visible viewport, not an ideal full-screen canvas.

Required behavior:

- At 100% browser zoom, no slide should require the user to zoom out to see all content.
- Desktop presentation mode should have no accidental page scroll.
- The active slide's title, primary content, footer/progress, and controls must all be visible at 1280x720.
- Add a low-height compact mode for content-dense decks:
  `@media (max-height: 820px) and (min-width: 921px)`.
- In compact mode, reduce slide padding, heading size, card min-heights, row heights, and diagram height before reducing body readability too far.
- Dense tables/lists must use shorter rows, smaller labels, or be split across slides. Do not rely on browser zoom.
- Fixed-format elements such as metric cards, architecture diagrams, flow cards, terminal panels, and image grids need explicit `max-height`, `min-height`, or responsive height constraints.
- Avoid content blocks with `min-height` values that exceed the remaining slide space after header and footer.
- If a slide still overflows after compacting, split it into two slides rather than shrinking everything into unreadability.

Useful pattern:

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
  .card, .metric, .flow-card { min-height: auto; padding: 12px; }
}
```

## Style Strategies

### Clean Product

Best for product demos, sales materials, internal briefings, SaaS workflows.

Use:

- light or calm dark surfaces
- restrained accent
- product mock panels
- dense but organized information
- practical, scannable layout

### Swiss Grid

Best for data-driven, technical, executive, or product strategy decks.

Use:

- 12/16-column feel
- very clear alignment
- light title weights when appropriate
- big numbers
- hairlines
- image evidence blocks

### Editorial Magazine

Best for narrative, brand, industry observation, launch story, founder pitch.

Use:

- stronger chapter rhythm
- image-led pages
- typographic contrast
- fewer but more memorable elements
- cover-like moments

### Case Preview / Notebook

Best for style discovery and lighter teaching or storytelling.

Use:

- distinctive but controlled visual metaphors
- visible tabs / sheets / preview-board patterns if useful
- strict viewport fitting
- no scrolling slides

## Typography

Resolve a typography preset before final generation. In direct generation, auto-select the preset. In style or template/case paths, ask only after the concrete style or concrete template/case has been selected:

- Hanbao Default: balanced product typography.
- Editorial Luxe: high-contrast serif titles, italic emphasis, premium B2B editorial feel.
- Swiss Light: lighter display weights and strict grid alignment.
- Editorial Serif: high contrast story-led typography.

Do not leave typography unresolved or always default to one preset.

## Speaker Notes

If the user asks for presentation help, speaker notes, script, presenter view, or teleprompter:

- Add hidden notes per slide.
- Notes should be speaker cues, not visible slide text.
- Visible slide copy must remain audience-facing.

## Generation Workflow

1. Determine input quality: topic only, rough notes, full content, existing deck, source files, or assets.
2. Create an audience-centered production brief: intended viewer, viewer need, desired takeaway, must-cover content, evidence, visual direction, image responsibilities, data needs, motion expectations, and output constraints. Use `references/query-rewrite.md` when needed.
3. If source files/folders/evidence are involved, use `slide-source-grounding` first.
4. Create a mandatory deck plan. Use `slide-narrative-planner` when content is messy, long, source-heavy, strategic, educational, or presentation-critical.
5. Decide choice mode: direct generate, style selection, or case selection.
6. Choose one primary path using the Style Path Routing table.
7. Define page transitions, per-slide entry choreography, and interaction feedback using `slide-motion-polish` at full power.
8. Load only the templates/references for that path.
9. Pick deck length and layout rhythm.
10. Confirm typography is resolved and apply that preset's tokens. For direct generation, this is the internally selected preset; for style/case paths, this is the user's chosen preset.
11. Write slide outline with one clear job per slide, unless `slide-narrative-planner` already supplied one.
12. Scan all slide jobs for numbers, comparisons, KPI cards, tables, charts, trends, timelines, matrices, dashboards, or structured evidence. If any are present, use `slide-data-visuals` now, before writing final HTML. This is the default non-image chart generation path. It must produce inline component specs that inherit the selected background, palette, typography, and motion tokens.
13. If image generation / host image generation is available and the user has not explicitly opted out, use `slide-visual-assets` now at max power, before writing final HTML, to generate multiple real image assets with distinct responsibilities. Do not treat prompts as deliverables.
14. In parallel with independent image generation and independent data-visual specs, prepare the selected path template, typography tokens, slide copy, and motion grammar. Do not write the final HTML until required image paths, no-image fallback decisions, chart/dashboard specs, and the motion plan are known.
15. Generate single-file HTML with the resolved data visuals, image paths, or no-image fallbacks already known.
16. Run visual audit and motion polish as parallel review passes when possible. They should produce findings, not edit the same HTML file concurrently.
17. Apply audit and motion fixes in one controlled integration pass.
18. Verify:
   - no blank slides
   - no overflow at desktop viewport
   - every slide is fully visible at normal browser zoom
   - 1280x720 and low-height browser windows do not hide bottom content
   - keyboard navigation works
   - direct slide link works
   - images render
   - charts and dashboards render inline, match the slide background/style, and require no external network assets
   - generated image paths or embedded base64 assets resolve correctly
   - console has no errors
19. If verification finds issues, patch once and re-run only the failed checks.

## Quality Rules

- One slide, one job.
- Split overloaded slides instead of shrinking everything.
- Avoid identical card grids across many consecutive slides.
- Images must be evidence, product context, or story anchor, not filler.
- Charts must be native slide components and visually belong to the selected background and style system.
- Avoid generic AI aesthetics: purple gradients, decorative blobs, fake metrics, icon spam.
- Prefer controlled tokens over ad hoc colors.
- Keep text readable from a presentation distance.
- Do not use visible text to explain how to use the slide deck.
- Do not leave placeholder slides, generic filler, or visible AI-process language.
