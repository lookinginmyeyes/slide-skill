---
name: hanbao-html-slide
description: >-
  Generate a polished single-file HTML slide deck for product, sales, pitch,
  report, teaching, or sharing presentations. Use when the user asks to
  create PPT, slides, deck, presentation, keynote, HTML deck, web PPT, HTML
  slides, or wants style selection or case selection before generation. This
  skill distills html-ppt, guizang-ppt-skill, frontend-slides, and the
  single-file-safe parts of huashu-design into one main generator. Output must
  always be one self-contained HTML file.
---

# Hanbao HTML Slide

Main generation skill for generic HTML slide decks.

This skill generates **one self-contained HTML file**. It may use uploaded or system asset URLs, but it must not create a React/Vite project, require a dev server, or output a multi-file artifact.

Important: do not reduce or remove useful upstream capabilities. This is an integration and routing skill, not a minimal rewrite. Keep the useful template/runtime/layout/theme/preview capabilities available, but load only the branch needed for the user's chosen style path.

Use the `slide-motion-polish` / `emil-design-eng` framework at full power during deck generation. It is not just a final cleanup pass. The deck skeleton, page transitions, slide entry choreography, local reveal effects, press/hover feedback, panel behavior, and visual rhythm should be designed with that framework before the HTML is written.

Keep preprocessing modular. If the input needs source extraction, story planning,
or image generation, call the matching independent module
instead of expanding this generator into a large all-purpose harness.

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

Do not "simplify away" the upstream strengths:

- Keep html-ppt's runtime, notes, presenter concepts, layouts, themes, and animation options.
- Keep guizang's Swiss and magazine visual systems, template discipline, and image-evidence rules.
- Keep guizang magazine's signature WebGL dual-background hero effect. When routing to guizang magazine/editorial, use `assets/guizang/template.html` as the skeleton and preserve `canvas#bg-dark`, `canvas#bg-light`, shader code, `startGL()`, hero low-opacity overlays, and light/dark background switching.
- Keep frontend-slides' style preview, viewport fitting, density limits, and animation richness.
- Keep the single-file-safe parts of huashu-style high-fidelity HTML, especially visual anchors, typography feel, and anti-generic-design discipline.
- Keep emil-design-eng's full design engineering force for art direction, layout refinement, motion purpose, easing, transition feel, and invisible interaction details.

The integration layer decides **which branch to use**, not which features to delete.

## Companion Modules

Use these modules as needed before or around generation:

| Situation | Module | What it returns |
| --- | --- | --- |
| User provides local files, reports, docs, PDFs, spreadsheets, or wants citations/evidence | `slide-source-grounding` | evidence pack with source locations |
| User provides messy notes/topic/article/script or needs stronger logic | `slide-narrative-planner` | deck plan, action titles, slide jobs, click beats |
| Image2 / GPT Image 2 / host-native image generation is available and the user has not opted out | `slide-visual-assets` | generated image paths, placement notes, and fallback decisions before final HTML writing |

Do not load these modules for every run. Load the module only when the user's
input or selected direction needs that capability.

### From html-ppt

Use as the stable backbone:

- token-based theme system
- reusable layout registry
- keyboard runtime
- overview / progress / presenter notes concepts
- audience-facing slide content separated from speaker-only notes

Bundled resources kept from html-ppt:

- `assets/html-ppt/base.css`
- `assets/html-ppt/runtime.js`
- `assets/html-ppt/fonts.css`
- `assets/html-ppt/animations/`
- `assets/html-ppt/single-page/`
- selected theme CSS files in `assets/html-ppt/`
- `references/html-ppt/themes.md`
- `references/html-ppt/layouts.md`
- `references/html-ppt/animations.md`
- `references/html-ppt/presenter-mode.md`
- `references/html-ppt/authoring-guide.md`

### From guizang-ppt-skill

Use as premium visual modes:

- Swiss grid mode
- editorial magazine mode
- WebGL dual-background fluid hero motion on magazine covers and chapter openers
- strong chapter openers
- big stat pages
- image-as-evidence blocks
- strict layout discipline instead of freeform decoration

Bundled resources kept from guizang:

- `assets/guizang/template.html`
- `assets/guizang/template-swiss.html`
- `assets/guizang/motion.min.js`
- `references/guizang/layouts.md`
- `references/guizang/layouts-swiss.md`
- `references/guizang/themes.md`
- `references/guizang/themes-swiss.md`
- `references/guizang/swiss-layout-lock.md`
- `references/guizang/image-prompts.md`
- `references/guizang/checklist.md`

### From frontend-slides

Use as the selection layer:

- "show, don't tell" style previews
- three distinct visual options when style is unclear
- viewport fitting rules
- density limits per slide
- zero-dependency single HTML delivery

Bundled resources kept from frontend-slides:

- `references/frontend-slides/viewport-base.css`
- `references/frontend-slides/STYLE_PRESETS.md`
- `references/frontend-slides/html-template.md`
- `references/frontend-slides/animation-patterns.md`
- `references/viewport-fit.md` for generic 100% browser zoom and low-height viewport fitting

### From huashu-design

Use only single-file-safe ideas:

- strong visual anchor before full production
- tasteful high-fidelity HTML feel
- CSS-only / vanilla JS motion
- anti-generic-design discipline

Bundled resources kept from huashu-design for single-file-safe high-fidelity direction:

- `references/huashu-design/original-SKILL.md`
- `references/huashu-design/design-styles.md`
- `references/huashu-design/workflow.md`
- `references/huashu-design/slide-decks.md`
- `references/huashu-design/critique-guide.md`
- `references/huashu-design/animation-best-practices.md`
- `references/huashu-design/animation-pitfalls.md`
- `references/huashu-design/verification.md`
- selected single-HTML demos in `references/huashu-design/`
- `assets/huashu/deck_index.html`
- `assets/huashu/deck_stage.js`

Do not inherit huashu multi-file workflows, React projects, video export pipelines, dev-server flows, or live tweak systems. If a retained upstream huashu reference mentions Tweaks, ignore that section for slide generation.

## Entry Choice Gate

Default behavior is **entry-choice-first**.

Before doing slide generation work or reading branch-specific resources, ask the user to choose one path:

1. Direct generation
2. Choose a style
3. Choose a template/case

Ask this even for ambiguous or topic-only requests. If the user's initial prompt already explicitly chooses one of these paths, follow it without asking again.

Direct generation is allowed only when the user chooses direct generation or clearly says things like "directly generate", "just make it", "no need to ask", "quick draft", "直接出", "不用问", "快速生成", or "一把梭".

Typography selection is mandatory and happens after the concrete route is chosen, but before final generation. Even if the user chooses direct generation, ask them to pick a typography preset before writing the deck.

Important: user-facing options are **not** internal branch names. When offering styles or cases, describe the visible direction in plain language. Do not tell the user which upstream skill, retained source, or branch will be used. The agent chooses that mapping internally.

The first question should be short, precise, and should not include implementation details. Example:

```text
这份 slide 你想怎么定方向？
1. 直接生成：我根据内容自动定风格、模板和动效
2. 先选风格：先看 2-3 个视觉方向，再生成
3. 先选模板/案例：先看 2-3 个成品结构方向，再生成
```

## Path Rules

Before loading branch resources, classify the request:

| User wording | Action |
| --- | --- |
| Explicit direct-generation wording | Generate immediately with an internally selected style path |
| Explicitly asks to choose style, or chooses entry path 2 | Offer 2-3 style options |
| Explicitly asks to choose examples/cases/templates, or chooses entry path 3 | Offer 2-3 template/case options |
| Ambiguous or topic-only request | Ask the three-path entry question first |

When offering options:

- Keep options short enough for the user to choose quickly.
- Use user-facing option names. Do not use source skill names, branch names, or implementation labels.
- Describe only visible output: visual tone, first-screen feel, content-page structure, motion feel, and best-use scenario.
- Each option should include one tradeoff or best-fit note so the user can make a real choice.
- Do not reveal the hidden branch or upstream skill behind each option.
- Do not read detailed branch assets yet. Use only the high-level routing guidance and the prompt.
- Wait for the user's choice before loading selected branch resources.

Style option format:

```text
选一个视觉方向：
1. [Name]：一句话描述画面气质。适合：...
2. [Name]：一句话描述画面气质。适合：...
3. [Name]：一句话描述画面气质。适合：...
```

Template/case option format:

```text
选一个模板/案例方向：
1. [Name]：开场页...，内容页...，动效...。适合：...
2. [Name]：开场页...，内容页...，动效...。适合：...
3. [Name]：开场页...，内容页...，动效...。适合：...
```

## Mandatory Typography Choice

Before final generation, ask the user to choose one typography preset. Do this only after the route is concrete:

- Direct generation path: ask typography after the user chooses direct generation.
- Style selection path: first offer 2-3 style options, wait for the user to pick one, then ask typography.
- Template/case path: first offer 2-3 template/case options, wait for the user to pick one, then ask typography.

1. Hanbao Default: clean sans typography for product, sales, and internal decks
2. Editorial Luxe: high-contrast serif headlines with italic emphasis for premium B2B storytelling
3. Swiss Light: light-weight sans typography for data-heavy, executive, and technical decks
4. Editorial Serif: quieter serif display for narrative, brand, and magazine-like decks

Rules:

- Do not silently choose a typography preset.
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
2. Pick one style strategy internally.
3. Ask for typography choice unless the user already selected one.
4. Pick a transition system and motion grammar using the full `slide-motion-polish` / `emil-design-eng` framework.
5. Route to exactly one primary branch from the routing table below.
6. Load only that branch's necessary templates/references.
7. Generate the full single-file HTML deck.
8. Run the default audit/polish chain if available.

### 2. Style Selection

Use when the user asks to pick a style or chooses entry path 2. If the prompt is ambiguous, ask the three-path entry question first instead of jumping here.

Offer 2-3 style strategies selected by the agent for the user's actual content and audience. Do not use a fixed menu every time. These options are chosen by the agent; the user does not need to know or choose the internal implementation branch.

Possible strategy pool:

- Clean Product: restrained product UI, clear hierarchy, quiet polish.
- Swiss Grid: strict grid, large type, data/evidence blocks.
- Editorial Magazine: serif or humanist display, image-led story, chapter rhythm.
- Warm Studio: high-fidelity product/demo feel, refined editorial-product typography, and single-file product storytelling.
- Technical Blueprint: structured engineering/report feel.
- Dark Product: focused screen/demo feel for technical or AI workflows.
- Notebook / Casebook: tactile example-led teaching or discovery.
- Investor Brief: crisp claims, metrics, ask, and narrative flow.

The selected style applies to the entire main generator. It does not mean "call only guizang" or "call only frontend-slides".

After the user selects a style, internally route directly to the matching branch. Do not read every retained file "just in case". Keep this mapping invisible unless the user explicitly asks for implementation details.

### 3. Case Selection

Use when the user wants examples or does not know the desired style.

Generate or describe 2-3 concrete mini previews with:

- title slide look
- one content page look
- motion feel
- typography feel
- best-use note

After selection, lock the visual direction and generate one complete single-file HTML deck.

Case selection is also only a selection layer. It must be able to propose cases from all retained style families, not only frontend-slides presets. Once the case is selected, internally route to the corresponding branch and continue there.

Case candidates may come from:

- html-ppt templates/themes for stable product, report, pitch, speaker-note decks.
- guizang Swiss or magazine templates for premium visual decks.
- frontend-slides presets for visual discovery and strict viewport-fit examples.
- Warm Studio / huashu-derived single-file demos for refined editorial-product typography and high-fidelity product/demo pages.

For case selection:

1. Infer 2-3 distinct cases that fit the user's content.
2. For each case, describe only the visible result: page feel, structure, typography, motion, and best-use note.
3. Keep the branch/source-skill mapping internal.
4. After selection, read only that chosen branch's resources.

## Branch Routing

Pick one primary branch internally after direct/style/case selection. Load only that branch's resources first. Load another branch only if a slide genuinely needs a capability that the primary branch lacks.

| User choice / deck need | Primary branch | Read these resources first | Do not read by default |
| --- | --- | --- | --- |
| Standard product/report/pitch deck | html-ppt | `assets/html-ppt/base.css`, `assets/html-ppt/runtime.js`, `assets/html-ppt/single-page/`, `references/html-ppt/layouts.md`, one selected theme CSS | guizang templates, all frontend style presets |
| Speaker notes / presenter mode | html-ppt presenter | `references/html-ppt/presenter-mode.md`, `assets/html-ppt/runtime.js`, relevant layouts | guizang templates unless premium visual mode is selected |
| Swiss grid / executive / data-driven | guizang Swiss | `assets/guizang/template-swiss.html`, `references/guizang/layouts-swiss.md`, `references/guizang/themes-swiss.md`, `references/guizang/swiss-layout-lock.md` | html-ppt theme catalog, frontend preview docs |
| Editorial / magazine / launch story | guizang magazine | `assets/guizang/template.html`, `references/guizang/layouts.md`, `references/guizang/themes.md`, `references/guizang/image-prompts.md` | Swiss lock unless user chose Swiss; do not use html-ppt skeleton for this branch |
| User wants 2-3 visual examples first | case preview router | select 2-3 candidates across html-ppt / guizang / frontend / Warm Studio, then read only the chosen branch | all unchosen branch catalogs |
| Strict viewport-fit single HTML | frontend constraints | `references/frontend-slides/viewport-base.css`, `references/frontend-slides/html-template.md` | full theme catalogs |
| Dense slide / low-height browser fit | browser viewport fit | `references/viewport-fit.md`, then selected branch layout rules | unrelated branch catalogs |
| Premium B2B editorial typography | Warm Studio or selected branch + Editorial Luxe | `references/huashu-design/design-styles.md`, `references/huashu-design/workflow.md`, `assets/huashu/deck_index.html`, plus selected typography tokens | unrelated branch catalogs |

Routing rules:

- One branch owns the deck skeleton.
- If guizang magazine owns the deck skeleton, preserve the original dual WebGL background system and hero overlay behavior from `assets/guizang/template.html`; do not flatten it into a static background.
- Other branches may contribute a specific feature, such as viewport rules or image asset guidance, but must not cause a full cross-branch rewrite.
- The motion system is always designed deliberately, even when a branch supplies a default runtime.
- Do not mix guizang Swiss and guizang magazine class systems in the same deck.
- If using html-ppt runtime with guizang-inspired visuals, keep html-ppt as runtime owner and import only the visual rules needed.
- If using guizang template as skeleton, do not replace its navigation with html-ppt runtime unless explicitly requested.
- If using Warm Studio / huashu-derived direction, preserve the single-file HTML output contract and do not import React/Vite/video-export workflows.

## Motion And Art Direction

Use `slide-motion-polish` as a generation-time design partner, not just a reviewer.

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
- When a slide is dense, executive, or data-heavy, motion should be restrained and clarify reading order.
- Use tabular numbers and stable text containers so animated digits or headline motion do not cause layout jitter.
- For interactive decks, design component states before coding: hover, active, focus-visible, selected/current, disabled, and loading where relevant.
- Do not add fake interactive controls. Every control should either perform an action or be rendered as non-interactive content.

## Layout Registry

When implementing a deck, prefer copying from bundled layout sources before inventing markup:

1. For standard template-driven decks, inspect `assets/html-ppt/single-page/` and `references/html-ppt/layouts.md`.
2. For Swiss premium pages, inspect `assets/guizang/template-swiss.html` and `references/guizang/layouts-swiss.md`.
3. For editorial premium pages, inspect `assets/guizang/template.html` and `references/guizang/layouts.md`.
4. For strict viewport fitting, apply the rules from `references/frontend-slides/viewport-base.css`.

Only inspect the sources needed for the selected branch.

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
- process / pipeline
- image hero
- image evidence grid
- quote
- closing

Do not invent many one-off layouts unless the content shape truly requires it.

## Viewport-Fit Contract

HTML decks are viewed inside browsers, product previews, and embedded surfaces.
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

Always ask the user to choose a typography preset before final generation unless they already selected one explicitly. In style or template/case paths, ask only after the concrete style or concrete template/case has been selected:

- Hanbao Default: balanced product typography.
- Editorial Luxe: high-contrast serif titles, italic emphasis, premium B2B editorial feel.
- Swiss Light: lighter display weights and strict grid alignment.
- Editorial Serif: high contrast story-led typography.

Do not silently default to any preset.

## Speaker Notes

If the user asks for presentation help, speaker notes, script, presenter view, or teleprompter:

- Add hidden notes per slide.
- Notes should be speaker cues, not visible slide text.
- Visible slide copy must remain audience-facing.

## Generation Workflow

1. Determine input quality: topic only, rough notes, full content, existing deck, source files, or assets.
2. If source files/folders/evidence are involved, use `slide-source-grounding` first.
3. If the content needs structure, use `slide-narrative-planner` to create a deck plan.
4. Decide choice mode: direct generate, style selection, or case selection.
5. Choose one primary branch using the Branch Routing table.
6. Define page transitions, per-slide entry choreography, and interaction feedback using `slide-motion-polish`.
7. Load only the templates/references for that branch.
8. Pick deck length and layout rhythm.
9. Confirm the user has chosen a typography preset and apply that preset's tokens.
10. Write slide outline with one clear job per slide, unless `slide-narrative-planner` already supplied one.
11. If image2 / GPT Image 2 / host-native image generation is available and the user has not explicitly opted out, use `slide-visual-assets` now, before writing final HTML, to generate actual image files or return fallback decisions. Do not treat prompts as deliverables.
12. In parallel with independent image generation, prepare the selected branch template, typography tokens, slide copy, and motion grammar. Do not write the final HTML until required image paths or fallback decisions are known.
13. Generate single-file HTML with the resolved image paths or no-image fallbacks already known.
14. Run visual audit and motion polish as parallel review passes when possible. They should produce findings, not edit the same HTML file concurrently.
15. Apply audit and motion fixes in one controlled integration pass.
16. Verify:
   - no blank slides
   - no overflow at desktop viewport
   - every slide is fully visible at normal browser zoom
   - 1280x720 and low-height browser windows do not hide bottom content
   - keyboard navigation works
   - direct slide link works
   - images render
   - generated image paths or embedded base64 assets resolve correctly
   - console has no errors
17. If verification finds issues, patch once and re-run only the failed checks.

## Quality Rules

- One slide, one job.
- Split overloaded slides instead of shrinking everything.
- Avoid identical card grids across many consecutive slides.
- Images must be evidence, product context, or story anchor, not filler.
- Avoid generic AI aesthetics: purple gradients, decorative blobs, fake metrics, icon spam.
- Prefer controlled tokens over ad hoc colors.
- Keep text readable from a presentation distance.
- Do not use visible text to explain how to use the slide deck.
