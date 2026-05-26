---
name: slide-motion-polish
description: >-
  Plan, design, audit, and improve motion, interaction feel, animation purpose,
  easing, duration, hover/active feedback, slide navigation, page transitions,
  slide entry choreography, panels, presenter controls, layout rhythm, visual
  polish, and reduced-motion handling in single-file HTML slide decks. Use
  during generation and again after visual audit when the user asks to optimize
  animation, interaction, micro-interactions, polish, motion, transitions, page
  switching, layout, or overall feel. This skill preserves the full
  motion-craft design engineering framework and should run at full power.
---

# Slide Motion Polish

Full-power motion, interaction, and visual polish module for HTML slide decks.

This skill runs at full power by default. It should run twice when possible:

1. During generation, before HTML is written, to plan the deck's transition system, entry choreography, interaction feel, and visual rhythm.
2. After visual audit, to tune timing, easing, layout harmony, responsiveness, and reduced-motion behavior.

Do not use motion to hide weak hierarchy. If the layout is weak, improve the layout and then animate the improved structure. Do not silently downgrade to static/fade-only output unless the user explicitly asks for low motion, the deck is purely archival, or the environment cannot support safe verification.

Bundled references:

- `references/motion-craft/full-reference.md`
- `references/motion-design.md`
- `references/interaction-design.md`
- `references/animate.md`
- `references/delight.md`
- `references/optimize.md`

Load these only when the motion or interaction problem needs more detail than the core checklist below.

The full original `motion-craft` skill is retained in `references/motion-craft/full-reference.md`. Use it as the authoritative reference for full-power behavior, especially when shaping page transitions, dynamic effects, component feel, and visual polish.

Do not bulk-read every motion reference. Route by problem:

| Problem | Read |
| --- | --- |
| Animation purpose / timing / easing | `motion-design.md` |
| Buttons, panels, controls, interaction state | `interaction-design.md` |
| Adding purposeful animation | `animate.md` |
| Delightful but still useful details | `delight.md` |
| Performance or sluggish interaction | `optimize.md` |

Keep the full motion-craft design engineering framework intact. The routing only controls context loading, not capability reduction.

## Core Principle

Every animation must earn its place. Motion should clarify state, preserve spatial continuity, provide feedback, or reduce jarring changes. If motion only says "look at me", remove or reduce it.

For slide decks, full-power use means:

- page switching has a designed transition system, not a default fade
- dynamic effects support the narrative, charts, diagrams, comparisons, or product screenshots
- layout and motion are planned together
- pressable controls feel responsive
- repeated navigation stays fast
- visual polish is visible in spacing, alignment, timing, easing, and state changes

## Generation-Time Planning

Before writing the deck HTML, define these decisions:

| Layer | Decision |
| --- | --- |
| Page transition | horizontal push, depth shift, masked reveal, calm dissolve, or instant for dense/rapid decks |
| Timing | default 180-360ms for slide changes; longer only for rare chapter/title moments |
| Easing | use strong custom curves, not weak default `ease` for major motion |
| Entry choreography | title/body/media/chart/accent order and delay rhythm |
| Local reveal | counters, numeric morphs, kinetic typography, chart draw-ins, diagram strokes, comparison highlights, spotlight masks |
| Interaction feedback | active scale, hover shift, focus ring, selected state, disabled state, panel open/close, tooltip/popover behavior |
| Layout polish | align motion direction with composition, grid, and reading order |
| Reduced motion | equivalent non-animated state for every transition and reveal |

Choose a motion grammar that matches the deck:

| Deck type | Motion grammar |
| --- | --- |
| Product/demo | polished depth, masked screenshot reveals, small parallax, responsive controls |
| Executive/report | short directional transitions, subtle chart draw-ins, stable reading order |
| Editorial/story | chapter-level cinematic transitions, image reveals, restrained text choreography |
| Technical/architecture | diagram build-ups, line drawing, node highlights, code/terminal reveals |
| Teaching/casebook | step-by-step reveals, stable prior context, clear highlight motion |

The chosen grammar should be consistent enough to feel designed, but not so repetitive that every slide performs the same animation.

## Kinetic Type And Numeric Motion

For slides where the headline, metric, date, price, score, percentage, or ranking is the visual anchor, design a specific type/number motion moment. This can happen during page transitions or local reveals.

Treat page-transition numeric motion as a signature pattern. When a slide opens with a KPI, percentage, price, score, ranking, date, or version number, strongly consider a short digit-roll, count-up, or number-morph moment as part of the page transition. This is especially important for hero stat slides, data reveal slides, comparison slides, and executive summary pages.

Use patterns such as:

| Pattern | Use |
| --- | --- |
| Headline lift / snap | chapter openers, key claims, product statements |
| Word cascade | short editorial titles or narrative beats |
| Weight/scale pulse | emphasizing one key term without moving the whole layout |
| Counter count-up | KPI, revenue, conversion, growth, time saved |
| Digit roll / odometer | dates, rankings, versions, step numbers, score changes |
| Number morph | before/after metrics, comparison slides, forecast changes |
| Label lock-in | small captions or axis labels settling after a chart reveal |

Rules:

- Use kinetic type and numeric motion as a designed accent, not on every slide.
- For hero/stat slides, prefer numeric motion during the page transition unless it would distract from readability.
- For before/after slides, prefer number morph over a simple fade.
- For dates, rankings, versions, and step numbers, prefer digit-roll / odometer motion.
- Never create accidental text jitter. Reserve enough width, use `font-variant-numeric: tabular-nums`, and animate `transform`/`opacity` rather than layout properties.
- For variable fonts, subtle weight or optical-size shifts are allowed if they do not cause reflow.
- Keep page-transition type motion short: usually 180-360ms, with a slightly longer chapter-title moment only when rare.
- Numeric changes should end on an exact, readable value and should not keep ticking after the slide settles.
- Dense report slides should use restrained number changes; hero/stat slides can use a more expressive count-up or digit roll.
- Reduced-motion mode must show the final text/number immediately.

## Review Format

When reviewing code, use a markdown table:

| Before | After | Why |
| --- | --- | --- |

Use this for motion and interaction findings. Do not use loose Before/After paragraphs.

## Animation Decision Framework

### 1. Should this animate?

Ask how often the user sees it:

| Frequency | Decision |
| --- | --- |
| 100+ times/day, keyboard shortcuts, rapid navigation | no visible animation or extremely reduced |
| tens of times/day, hover/list navigation | remove or keep very subtle |
| occasional, panels, notes, drawers, toasts | standard animation |
| rare/first-time, onboarding or celebration | can be more expressive |

Deck-specific rule:

- Keyboard slide navigation should feel immediate. Keep transitions short and never block rapid navigation.
- Richer transitions are allowed for chapter/title moments, but ordinary slide-to-slide navigation must stay fast.

### 2. What is the purpose?

Valid purposes:

- spatial consistency
- state indication
- explanation
- feedback
- preventing jarring changes

Invalid purpose:

- "it looks cool" on repeated interactions

### 3. What easing?

Use custom curves:

```css
--ease-out: cubic-bezier(0.23, 1, 0.32, 1);
--ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);
--ease-drawer: cubic-bezier(0.32, 0.72, 0, 1);
```

Rules:

- Entering elements: ease-out.
- Moving/morphing elements: ease-in-out.
- Hover/color change: ease.
- Constant motion: linear.
- Avoid `ease-in` for UI entrances.

### 4. How fast?

| Element | Duration |
| --- | --- |
| button press feedback | 100-160ms |
| tooltip / small popover | 125-200ms |
| dropdown / select | 150-250ms |
| slide transition | 180-360ms |
| modal / drawer / notes panel | 200-500ms |
| explanatory marketing animation | can be longer, but must be rare |

## Required Checks

Flag and fix:

- `transition: all`
- `transform: scale(0)` entrances
- slow keyboard navigation
- heavy animations on every slide
- all elements using the same fade-up
- bounce/elastic motion in serious UI
- animating layout properties like width, height, top, left when transform would work
- missing `:active` feedback on buttons
- missing hover/focus affordances on controls
- no `prefers-reduced-motion` handling
- motion that changes layout and causes text overlap

## Interaction Details

Design component interactions as part of the deck, not as browser-default controls. Every interactive element should feel intentional but should not distract from presenting.

Before implementing, identify the components that exist in the deck:

- slide navigation buttons
- progress bar or chapter indicator
- thumbnails / overview grid
- speaker notes or presenter panel
- toggles, sliders, segmented controls
- tabs, accordions, filters, comparison controls
- clickable hotspots on product screenshots
- tooltips, popovers, callouts, copy buttons
- chart legends, highlighted series, expandable annotations

For each component, define a state map:

| State | Required design response |
| --- | --- |
| default | clear affordance, enough contrast, aligned with deck style |
| hover | subtle lift, color shift, underline, indicator, or surface change |
| active/pressed | immediate scale or compression feedback, usually 100-160ms |
| focus-visible | keyboard-visible ring or outline that matches the visual system |
| selected/current | persistent indicator, not only color |
| disabled | visibly inactive, no misleading hover/press animation |
| loading/processing | short, calm feedback only if the action actually waits |

Component motion patterns:

- Buttons and icon controls: `scale(.97)` on press, hover shift or surface tint, no `transition: all`.
- Tabs / segmented controls: moving underline or thumb with transform, 160-240ms ease-out.
- Toggles: thumb movement with transform, clear on/off colors, no layout shift.
- Sliders: responsive thumb, track fill, keyboard support when possible.
- Tooltips/popovers: appear from the trigger origin, 125-200ms, no scale from zero.
- Panels/drawers: open from a meaningful edge, close slightly faster than open, restore focus.
- Hotspots: pulse only once or very subtly; on hover/click reveal a callout without shifting the slide.
- Chart legends: hover highlights the related mark and dims unrelated marks if useful.
- Overview/thumbnails: selected slide is obvious; hover should not resize the grid.

Rules:

- Interactions must be usable with mouse and keyboard where the component is actionable.
- Do not add fake controls that appear interactive but do nothing.
- Keep interaction code single-file-safe: vanilla JS and CSS are preferred.
- Use ARIA labels for icon-only controls.
- Respect `prefers-reduced-motion`.
- Never let hover/active/selected states resize buttons, cards, grids, or slide content.

### Buttons and Pressable Controls

Add subtle active feedback:

```css
.button {
  transition: transform 140ms var(--ease-out), background 180ms ease;
}
.button:active {
  transform: scale(.97);
}
```

### Panels and Presenter Controls

Panels should:

- open from a spatially meaningful location
- use ease-out or drawer curve
- close slightly faster than they open
- preserve focus where possible
- avoid blocking keyboard navigation unless intentionally modal

### Slide Transitions

Prefer:

- opacity + transform
- short duration
- consistent direction
- no scrollIntoView for iframe-embedded environments unless the runtime is built around scroll pages
- a small set of transition variants tied to slide role, such as normal page, chapter opener, data reveal, product demo

Avoid:

- long cinematic transitions on every key press
- heavy blur on dense text
- layout-shifting transitions
- identical default fade-up for the whole deck

Recommended transition families:

| Transition | Use |
| --- | --- |
| Directional push | most slide-to-slide movement; clear spatial continuity |
| Depth lift | product/demo slides where screenshots or panels are primary |
| Masked reveal | chapter openers, image-led stories, before/after comparisons |
| Calm dissolve | dense text/report slides where movement would distract |
| Instant/reduced | rapid navigation, keyboard-heavy review, reduced-motion mode |

### Step / Narration Slides

If the deck has step-by-step narration:

- animate only the new step or changed relationship
- keep prior context stable
- bind motion to meaning, such as draw line, reveal node, count number, highlight term
- do not reanimate the entire slide every step

## Reduced Motion

Always include:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation: none !important;
    transition: none !important;
  }
}
```

If the deck has a user motion toggle, respect both the user toggle and OS preference.

## Default Post-Audit Workflow

1. Read the HTML/CSS/JS motion code.
2. Identify page transitions, slide entry choreography, local reveal effects, navigation, controls, panels, and presenter UI.
3. Apply the full animation decision framework.
4. Patch unsafe, sluggish, generic, or visually weak motion.
5. Improve layout/motion alignment where animation exposes awkward spacing or hierarchy.
6. Ensure reduced-motion support.
7. Verify syntax and browser behavior when available.

## Success Criteria

- Navigation feels immediate.
- Controls feel responsive.
- Motion has purpose.
- No animation is trying to compensate for weak layout.
- The deck remains professional after animations are disabled.
