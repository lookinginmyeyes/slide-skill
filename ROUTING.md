# Routing Rules

The distilled skills keep retained capabilities, but should not load every resource for every run.

## Main Principle

User style or case choice selects a primary implementation path. The selected path owns the deck skeleton. Other paths can contribute specific rules, but must not trigger a full cross-resource read or rewrite.

User-facing choices should be framed around the viewer experience: visual direction, information density, use case, and communication goal. The agent chooses the implementation mapping.

Keep the harness thin. The root package routes to independent modules; the modules carry the durable behavior. Do not merge their instructions into one giant generation path.

Default behavior is entry-choice-first. Before loading detailed resources, create an audience-centered brief, then ask the user to choose one of three paths: direct generation, style selection, or template/case selection. If the user's initial prompt already explicitly chooses one of these paths, follow that path.

Typography must be resolved before final generation. Direct generation resolves typography automatically. Style selection and template/case selection ask the user for typography only after the user has picked a specific style option or a specific template/case option.

## Hard Stop Gates

Stop before final HTML generation whenever any applicable item is missing:

- entry path is not explicit and the user has not chosen direct/style/case
- style or template/case path was chosen but no concrete option has been selected
- typography preset has not been resolved; for direct generation this means auto-selected by the agent, for style/case paths this means selected by the user
- audience-centered brief and slide-job plan do not exist
- chart/KPI/table/dashboard needs have not been scanned and routed
- visual assets have not been generated or intentionally skipped in the full image-enabled package
- motion plan has not been created before HTML writing
- post-draft visual audit, motion polish, and viewport-fit verification have not run

## Full-Power Defaults

Unless the user explicitly asks for a simple/static/low-motion/no-image/rough draft:

- use the richest suitable visual direction for the audience and subject
- treat motion as part of the design system, not decoration
- use chart/dashboard components whenever structured data would make the point clearer
- in the full image-enabled package, use multiple generated assets with different slide responsibilities
- run audit and motion polish before final delivery
- verify that the result looks complete at normal browser zoom

Before the choice gate, only run conditional preprocessing when the input demands it:

- source files/folders/evidence -> `slide-source-grounding`
- raw messy content, long notes, article, script, pitch/research structure -> `slide-narrative-planner`
- chart/KPI/table/dashboard needs -> `slide-data-visuals` after style and typography are known; this is the default non-image chart generation path

Visual asset generation is different: when the environment supports image generation,
Raster Engine, or host image generation, treat `slide-visual-assets` as a
mandatory max-power default after the deck plan, selected direction, and
typography are known. It should produce multiple images with distinct slide
responsibilities. Skip only when the user explicitly says not to use generated
images, asks for a text-only / no-image deck, or the environment has no usable
image generation capability.

Motion is also max-power by default. Use `slide-motion-polish` during planning
and after audit unless the user explicitly asks for static/low-motion output or
the environment cannot support the necessary checks.

## Choice Gate

Before reading path-specific resources:

| Request type | User-facing response | Resource rule |
| --- | --- | --- |
| Ambiguous or topic-only request | Ask direct generation vs style selection vs template/case selection | Use this routing file only; do not read path assets yet |
| Explicit direct generation or user chooses direct generation | Generate immediately | Pick one implementation path, then read only that path |
| Explicit style selection or user chooses style selection | Offer 2-3 style directions | Use this routing file only; do not read path assets yet |
| Explicit template/case selection or user chooses template/case selection | Offer 2-3 concrete template/case previews | Use this routing file only; do not read path assets yet |

Direct generation requires clear wording such as "directly generate", "just make it", "no need to ask", "quick draft", "直接出", "不用问", "快速生成", or "一把梭".

The first entry question must be user-facing, precise, and simple:

```text
这份 slide 你想怎么定方向？
1. 直接生成：我根据内容自动定风格、模板和动效
2. 先选风格：先看 2-3 个视觉方向，再生成
3. 先选模板/案例：先看 2-3 个成品结构方向，再生成
```

Then:

- If the user chose direct generation, skip the typography question and auto-select the best-fitting typography preset.
- If the user chose style selection, ask for typography only after the user picks one concrete style option.
- If the user chose template/case selection, ask for typography only after the user picks one concrete template/case option.

## Style Paths

| Choice | Style path | Primary resources |
| --- | --- | --- |
| Standard product/report/pitch | studio-deck | `hanbao-html-slide/assets/studio-deck/`, `references/studio-deck/layouts.md` |
| Speaker notes / presenter mode | Studio Deck presenter | `references/studio-deck/presenter-mode.md`, `assets/studio-deck/runtime.js` |
| Swiss grid | Editorial Motion Swiss | `assets/editorial-motion/template-swiss.html`, `references/editorial-motion/layouts-swiss.md`, `references/editorial-motion/swiss-layout-lock.md` |
| Editorial / magazine | Editorial Motion magazine | `assets/editorial-motion/template.html`, `references/editorial-motion/layouts.md`, `references/editorial-motion/image-prompts.md` |
| Warm Studio / refined product demo | Warm Studio single HTML | `references/warm-studio/design-styles.md`, `references/warm-studio/workflow.md`, `assets/warm-studio/deck_index.html` |
| 2-3 visual examples first | case preview router | choose candidates across studio-deck, editorial-motion, viewport-showcase, and Warm Studio, then load only the selected path |
| Strict viewport fitting | frontend constraints | `references/viewport-showcase/viewport-base.css`, `references/viewport-showcase/html-template.md` |
| Dense slide / normal-browser 100% zoom fit | Hanbao viewport fit | `hanbao-html-slide/references/viewport-fit.md` |

## Style Selection Is Dynamic

Do not show the same fixed styles every time. The agent should infer the best 2-3 options from:

- audience
- deck purpose
- content density
- need for notes/presenter mode
- product/demo vs narrative/report
- visual risk tolerance
- asset availability

The 2-3 options are recommendations, not hardcoded categories.

When the user asks to choose by style:

1. The agent proposes 2-3 visible style directions tailored to the prompt.
2. The options should be named for the design result, not for the underlying skill.
3. The agent privately maps each option to the most suitable path.
4. After the user picks one, the agent loads only that path's needed resources.

During option generation, do not inspect `assets/` or deep `references/` files. Use the user's prompt and this routing table only. Detailed path files are loaded after the user picks.

Style options should be concise and decision-oriented:

```text
选一个视觉方向：
1. [Name]：画面气质...。适合：...
2. [Name]：画面气质...。适合：...
3. [Name]：画面气质...。适合：...
```

## Case Preview Includes All Families

Case preview is not the viewport-showcase path. It is a selector that can propose:

- an studio-deck case
- a Editorial Motion Swiss case
- a Editorial Motion magazine case
- a viewport-showcase style-preview case
- a Warm Studio / Warm Studio single HTML case

After the user picks one, continue with the implementation path that best produces that viewer-facing case and do not bulk-read the others.

When showing cases, describe the case's title slide, content page, motion feel, typography feel, and best-use scenario.

During case preview, do not inspect the detailed case source files yet. Describe plausible previews from the routing table and prompt, then load the chosen path only after selection.

Template/case options should describe structure, not just mood:

```text
选一个模板/案例方向：
1. [Name]：开场页...，内容页...，动效...。适合：...
2. [Name]：开场页...，内容页...，动效...。适合：...
3. [Name]：开场页...，内容页...，动效...。适合：...
```

## Quality Skills

## Editorial Motion Signature Rule

When the selected implementation path is `Editorial Motion magazine`, the deck skeleton must
come from `hanbao-html-slide/assets/editorial-motion/template.html`, not from an studio-deck
skeleton with editorial-motion-like colors.

Preserve these signature effects unless the user explicitly asks for a static or
low-power deck:

- `canvas#bg-dark` and `canvas#bg-light`
- `.slide.hero` low-opacity overlay rules that let the WebGL background show
- `FS_DARK` / `FS_LIGHT` shader blocks
- `bootGL()`, `startGL()`, `stopGL()`, and the `ppt-low-power-change` listener
- light/dark background switching through `body.light-bg`
- hero entrance motion from the original template

If these are removed, the output is only "magazine-inspired", not a true editorial-motion
magazine style-path result.

`slide-visual-audit` and `slide-motion-polish` also route by issue:

- visual hierarchy: visual audit `critique.md` / `polish.md`
- responsive overflow: visual audit `responsive-design.md`
- copy clarity: visual audit `ux-writing.md`
- animation timing/easing: motion polish `motion-design.md`
- controls/panels/buttons: motion polish `interaction-design.md`
- performance: motion polish `optimize.md`

`slide-motion-polish` is not only a post-generation fixer. For every generated deck, use it during planning to define page transitions, slide entry choreography, local reveal effects, interaction feedback, and visual rhythm. Treat the retained `motion-craft` framework as a full-power art-direction and motion-design layer.

Use the full original motion-craft reference when:

- the deck needs distinctive page switching
- the user asks for dynamic effects, premium feel, or stronger polish
- slide transitions feel flat, generic, or too similar
- buttons, panels, notes, or controls feel inert
- layout rhythm and motion need to be tuned together

## Conditional Modules

Use these modules only when their trigger is present. They are independent skills,
not hidden sections of the main generator.

| Need | Module | Load first | Output passed forward |
| --- | --- | --- | --- |
| Deck based on files, folders, PDFs, docs, sheets, or knowledge-base material | `slide-source-grounding` | `slide-source-grounding/SKILL.md` | Evidence pack with source locations |
| Raw idea needs story, outline, action titles, evidence hierarchy, or click beats | `slide-narrative-planner` | `slide-narrative-planner/SKILL.md` | Deck plan and slide jobs |
| Slides need charts, KPI cards, comparison tables, heatmaps, or dashboard pages | `slide-data-visuals` | `slide-data-visuals/SKILL.md` | Non-image inline HTML/SVG/CSS/JS chart specs that inherit the selected deck style |
| Environment supports image generation / host image generation and user has not opted out | `slide-visual-assets` | `slide-visual-assets/SKILL.md` | Generated image files, placement notes, and fallback decisions |

Do not load the detailed references for these modules unless the module's own
SKILL.md says the situation needs them.

## Pre-Generation Flow

```text
1. Build the audience-centered brief.
2. If source files/folders are involved, run source grounding.
3. Create a planning pass: slide count, slide jobs, evidence/data needs, visual role, and reveal rhythm.
4. Ask the required entry-choice question unless the user explicitly chose direct generation.
5. If style/case selection is chosen, offer 2-3 user-facing options without reading path assets, then wait for the concrete choice.
6. Resolve typography. For direct generation, auto-select the preset and continue. For style/case paths, ask typography after the concrete option is chosen and wait for the preset.
7. Scan every slide job for chart/KPI/table/dashboard opportunities. If any exist, run `slide-data-visuals` now. Data visuals must be inline slide components and must inherit the selected background, style tokens, typography, and motion system.
8. If image generation / host image generation is available and the user has not opted out, run `slide-visual-assets` now at max power to produce multiple real assets with distinct responsibilities, or fallback decisions only for failed/unnecessary assets.
9. Create the motion plan before final HTML: page transitions, slide entry choreography, local reveals, count-up/digit behavior, chart draw-ins, interaction feedback, and reduced-motion behavior.
10. Generate one single-file HTML deck with the selected path, resolved data visuals, resolved assets, and motion plan.
11. Run visual audit and motion polish; apply one controlled fix pass.
12. Verify normal-browser fit, keyboard navigation, direct slide links, no blank slides, and no bottom-content clipping.
```

## Parallelization Rules

Use parallelism where outputs do not mutate the same file:

| Stage | Can run in parallel | Must remain serial |
| --- | --- | --- |
| After deck plan | image briefs/assets per slide, path template reading, typography token preparation, motion grammar planning | final path selection |
| Data visuals | independent chart/dashboard/table specs per slide after style and typography are locked | writing the final HTML |
| Image generation | independent cover/chapter/product/evidence/diagram assets with unique output paths | writing the final HTML |
| First HTML draft review | visual audit and motion polish review passes | applying fixes to the HTML |
| Final QA | viewport screenshots/checks may be batched by viewport | mutation after QA findings |

If using subagents or parallel tool calls, assign non-overlapping ownership:

- Visual asset workers own asset generation requests, output files, and placement notes only.
- Audit workers own findings only.
- Motion workers own motion recommendations only.
- The main generator owns the final HTML file.

Never let two parallel workers edit the same HTML artifact.

## Non-Goals

- Do not collapse all retained skills into one tiny checklist.
- Do not bulk-read every retained file after the user chooses one style.
- Do not bulk-read every independent module for simple generation.
- Do not ship decks that require the user to zoom the browser out to see the full slide.
- Do not mix Editorial Motion Swiss and magazine class systems in one skeleton.
- Do not change the single-file HTML output contract.
- Do not add a live tweak panel to the final Hanbao slide deck.
- Final user-facing output should focus on the viewer and the finished artifact: what the deck is for, what it presents, where the HTML file is, and whether it passed verification.
