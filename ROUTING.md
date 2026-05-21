# Routing Rules

The distilled skills keep upstream capabilities, but should not load every resource for every run.

## Main Principle

User style or case choice selects a **primary branch internally**. The primary branch owns the deck skeleton. Other branches can contribute specific rules, but must not trigger a full cross-branch read or rewrite.

Do not expose branches, source skill names, or implementation mappings as user-facing choices. The user sees style/case options in normal design language. The agent chooses which retained source branch powers each option.

Keep the harness thin. The root package routes to independent modules; the modules carry the durable behavior. Do not merge their instructions into one giant generation path.

Default behavior is entry-choice-first. Before loading any detailed branch resources, ask the user to choose one of three paths: direct generation, style selection, or template/case selection. If the user's initial prompt already explicitly chooses one of these paths, follow that path.

Typography choice happens after the concrete route choice and before final generation. Do not ask typography before the user has picked a specific style option or a specific template/case option.

Before the choice gate, only run conditional preprocessing when the input demands it:

- source files/folders/evidence -> `slide-source-grounding`
- raw messy content, long notes, article, script, pitch/research structure -> `slide-narrative-planner`

Visual asset generation is different: when the environment supports image2,
GPT Image 2, or host-native image generation, treat `slide-visual-assets` as a
high-priority default after the deck plan, selected direction, and typography
are known. Skip only when the user explicitly says not to use generated images
or asks for a text-only / no-image deck.

## Choice Gate

Before reading branch-specific resources:

| Request type | User-facing response | Resource rule |
| --- | --- | --- |
| Ambiguous or topic-only request | Ask direct generation vs style selection vs template/case selection | Use this routing file only; do not read branch assets yet |
| Explicit direct generation or user chooses direct generation | Generate immediately | Pick one branch internally, then read only that branch |
| Explicit style selection or user chooses style selection | Offer 2-3 style directions | Use this routing file only; do not read branch assets yet |
| Explicit template/case selection or user chooses template/case selection | Offer 2-3 concrete template/case previews | Use this routing file only; do not read branch assets yet |

Direct generation requires clear wording such as "directly generate", "just make it", "no need to ask", "quick draft", "直接出", "不用问", "快速生成", or "一把梭".

The first entry question must be user-facing, precise, and simple:

```text
这份 slide 你想怎么定方向？
1. 直接生成：我根据内容自动定风格、模板和动效
2. 先选风格：先看 2-3 个视觉方向，再生成
3. 先选模板/案例：先看 2-3 个成品结构方向，再生成
```

Then:

- If the user chose direct generation, ask typography next.
- If the user chose style selection, ask for typography only after the user picks one concrete style option.
- If the user chose template/case selection, ask for typography only after the user picks one concrete template/case option.

## Branches

| Choice | Branch | Primary resources |
| --- | --- | --- |
| Standard product/report/pitch | html-ppt | `hanbao-html-slide/assets/html-ppt/`, `references/html-ppt/layouts.md` |
| Speaker notes / presenter mode | html-ppt presenter | `references/html-ppt/presenter-mode.md`, `assets/html-ppt/runtime.js` |
| Swiss grid | guizang Swiss | `assets/guizang/template-swiss.html`, `references/guizang/layouts-swiss.md`, `references/guizang/swiss-layout-lock.md` |
| Editorial / magazine | guizang magazine | `assets/guizang/template.html`, `references/guizang/layouts.md`, `references/guizang/image-prompts.md` |
| Warm Studio / refined product demo | huashu-derived single HTML | `references/huashu-design/design-styles.md`, `references/huashu-design/workflow.md`, `assets/huashu/deck_index.html` |
| 2-3 visual examples first | case preview router | choose candidates across html-ppt, guizang, frontend-slides, and Warm Studio, then load only the selected branch |
| Strict viewport fitting | frontend constraints | `references/frontend-slides/viewport-base.css`, `references/frontend-slides/html-template.md` |
| Dense slide / normal-browser 100% zoom fit | browser viewport fit | `hanbao-html-slide/references/viewport-fit.md` |

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
3. The agent privately maps each option to the most suitable branch.
4. After the user picks one, the agent loads only that branch's needed resources.

During option generation, do not inspect `assets/` or deep `references/` files. Use the user's prompt and this routing table only. Detailed branch files are loaded after the user picks.

Style options should be concise and decision-oriented:

```text
选一个视觉方向：
1. [Name]：画面气质...。适合：...
2. [Name]：画面气质...。适合：...
3. [Name]：画面气质...。适合：...
```

## Case Preview Includes All Families

Case preview is not the frontend-slides branch. It is a selector that can propose:

- an html-ppt case
- a guizang Swiss case
- a guizang magazine case
- a frontend-slides style-preview case
- a Warm Studio / huashu-derived single HTML case

After the user picks one, route to that branch and do not bulk-read the others.

When showing cases, describe the case's title slide, content page, motion feel, typography feel, and best-use scenario. Do not label the case as html-ppt, guizang, frontend-slides, or huashu-derived unless the user explicitly asks for internal details.

During case preview, do not inspect the detailed case source files yet. Describe plausible previews from the routing table and prompt, then load the chosen branch only after selection.

Template/case options should describe structure, not just mood:

```text
选一个模板/案例方向：
1. [Name]：开场页...，内容页...，动效...。适合：...
2. [Name]：开场页...，内容页...，动效...。适合：...
3. [Name]：开场页...，内容页...，动效...。适合：...
```

## Quality Skills

## Guizang Signature Rule

When the selected internal branch is `guizang magazine`, the deck skeleton must
come from `hanbao-html-slide/assets/guizang/template.html`, not from an html-ppt
skeleton with guizang-like colors.

Preserve these signature effects unless the user explicitly asks for a static or
low-power deck:

- `canvas#bg-dark` and `canvas#bg-light`
- `.slide.hero` low-opacity overlay rules that let the WebGL background show
- `FS_DARK` / `FS_LIGHT` shader blocks
- `bootGL()`, `startGL()`, `stopGL()`, and the `ppt-low-power-change` listener
- light/dark background switching through `body.light-bg`
- hero entrance motion from the original template

If these are removed, the output is only "magazine-inspired", not a true guizang
magazine branch result.

`slide-visual-audit` and `slide-motion-polish` also route by issue:

- visual hierarchy: visual audit `critique.md` / `polish.md`
- responsive overflow: visual audit `responsive-design.md`
- copy clarity: visual audit `ux-writing.md`
- animation timing/easing: motion polish `motion-design.md`
- controls/panels/buttons: motion polish `interaction-design.md`
- performance: motion polish `optimize.md`

`slide-motion-polish` is not only a post-generation fixer. For every generated deck, use it during planning to define page transitions, slide entry choreography, local reveal effects, interaction feedback, and visual rhythm. Treat the retained `emil-design-eng` framework as a full-power art-direction and motion-design layer.

Use the full original emil reference when:

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
| Environment supports image2 / GPT Image 2 / host-native image generation and user has not opted out | `slide-visual-assets` | `slide-visual-assets/SKILL.md` | Generated image files, placement notes, and fallback decisions |

Do not load the detailed references for these modules unless the module's own
SKILL.md says the situation needs them.

## Pre-Generation Flow

```text
1. If source files/folders are involved, run source grounding.
2. If structure is unclear or content is complex, run narrative planning.
3. Ask the required entry-choice question unless the user explicitly chose direct generation.
4. If style/case selection is chosen, offer 2-3 user-facing options without reading branch assets.
5. After a concrete style/case/direct path is locked, ask typography.
6. Create the deck plan and slide jobs.
7. If image2 / GPT Image 2 / host-native image generation is available and the user has not opted out, run `slide-visual-assets` now to produce actual assets or fallback decisions.
8. Generate one single-file HTML deck with the selected branch and resolved assets.
9. Run visual audit and motion polish.
```

## Parallelization Rules

Use parallelism where outputs do not mutate the same file:

| Stage | Can run in parallel | Must remain serial |
| --- | --- | --- |
| After deck plan | image briefs/assets per slide, branch template reading, typography token preparation, motion grammar planning | final branch selection |
| Image generation | independent cover/chapter/product/evidence/diagram assets with unique output paths | writing the final HTML |
| First HTML draft review | visual audit and motion polish review passes | applying fixes to the HTML |
| Final QA | viewport screenshots/checks may be batched by viewport | mutation after QA findings |

If using subagents or parallel tool calls, assign non-overlapping ownership:

- Visual asset workers own image prompts and image files only.
- Audit workers own findings only.
- Motion workers own motion recommendations only.
- The main generator owns the final HTML file.

Never let two parallel workers edit the same HTML artifact.

## Non-Goals

- Do not collapse all upstream skills into one tiny checklist.
- Do not bulk-read every retained file after the user chooses one style.
- Do not bulk-read every independent module for simple generation.
- Do not ship decks that require the user to zoom the browser out to see the full slide.
- Do not mix guizang Swiss and magazine class systems in one skeleton.
- Do not change the single-file HTML output contract.
- Do not add a live tweak panel to the final final slide deck.
