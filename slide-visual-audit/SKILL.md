---
name: slide-visual-audit
description: >-
  Audit and improve a single-file HTML slide deck for visual quality, UX
  clarity, responsive behavior, information hierarchy, typography, color
  strategy, layout rhythm, copy clarity, and anti-AI-template issues. Use after
  a deck is generated or when the user asks to review, polish, improve, audit,
  fix, beautify, or make slides more professional. This skill distills the audit
  and polish parts of visual-audit for slide decks and should run by default after
  generation.
---

# Slide Visual Audit

Default visual quality gate for generated HTML slide decks.

This skill reviews and improves the deck as a design artifact. It does not choose the primary generator. It operates after a deck exists.

## Scope

Use on:

- single-file HTML slide decks
- generated presentation pages
- embedded product demo decks
- pitch/report/teaching/share decks

Do not use for backend-only tasks or non-visual files.

Bundled references kept from the visual audit source material:

- `references/critique.md`
- `references/audit.md`
- `references/polish.md`
- `references/layout.md`
- `references/typeset.md`
- `references/responsive-design.md`
- `references/ux-writing.md`
- `references/color-and-contrast.md`
- `references/cognitive-load.md`

Load only the reference that matches the current problem. For example, use `responsive-design.md` for overflow/mobile issues, `ux-writing.md` for copy, and `color-and-contrast.md` for color problems.

Do not bulk-read every reference. Route by problem:

| Problem | Read |
| --- | --- |
| General review / scoring | `critique.md` |
| Production readiness / accessibility / errors | `audit.md` |
| Final quality pass | `polish.md` |
| Layout rhythm / spacing / hierarchy | `layout.md` |
| Font scale / readability | `typeset.md` |
| Mobile / viewport / overflow | `responsive-design.md` |
| Copy clarity | `ux-writing.md` |
| Palette / contrast | `color-and-contrast.md` |
| Too much information / cognitive burden | `cognitive-load.md` |

Keep the full retained audit capability available, but only load the needed path.

## Output Modes

### Audit Only

Return findings with file/slide references and recommended fixes.

### Audit + Fix

Patch the HTML/CSS/JS directly, then report what changed.

### Regression Gate

Check the deck after other edits and report whether it passes.

## Review Priorities

1. Broken or blank slides.
2. Text overlap, clipping, overflow, unreadable scale.
3. Slides that require browser zoom-out or shrinking the browser to see bottom content.
4. Weak information hierarchy.
5. Repetitive layout rhythm.
6. Generic AI visual patterns.
7. Poor color / typography choices.
8. Mobile and small-viewport failures.
9. Copy that repeats headings or explains the slide mechanics.

## Anti-Pattern Checks

Flag and fix:

- identical card grids used repeatedly
- decorative gradient text
- generic purple/blue gradients as default
- glassmorphism used as decoration
- fake metrics or meaningless numbers
- icon spam
- nested cards
- over-rounded generic cards
- visible "this slide shows..." presenter text
- images used as decoration with no evidence/story role
- all slides having the same structure

## Visual Hierarchy Checklist

For each slide:

- Is there one clear primary idea?
- Can the audience understand the page in 3-5 seconds?
- Is the most important object visually dominant?
- Are secondary details clearly secondary?
- Is there enough whitespace between title, body, and evidence?
- Does the page need to be split into two slides?

## Color Strategy

Choose or validate one of:

- Restrained: tinted neutrals plus one accent.
- Committed: one color carries a large part of the surface.
- Full palette: 3-4 semantic roles.
- Drenched: the whole surface is intentionally color-led.

Rules:

- Do not default to category cliches.
- Do not use pure `#000` or `#fff` when softer neutrals fit better.
- Avoid one-note palettes unless intentionally strict.
- Accent color should guide attention, not decorate every card.

## Typography

Check:

- heading/body scale contrast
- line-height
- line length
- Chinese-English mixed text
- label tracking
- whether title size fits the container
- whether meta text is too loud

Fix by changing tokens first, not one-off inline styles.

## Layout Rhythm

Check:

- no 3+ pages with the same grid rhythm unless deliberate
- no nested cards
- no page sections pretending to be floating cards
- stable dimensions for fixed-format elements
- bottom navigation / progress does not cover content
- image slots have aspect-ratio constraints

## Copy Clarity

Visible slide copy should be audience-facing.

Fix:

- remove restated headings
- shorten long paragraphs
- move speaker-only explanations into notes
- replace abstract claims with concrete phrasing
- keep labels short and useful

## Responsive Checks

At minimum reason about:

- 16:9 desktop
- 1280x720
- low-height desktop/laptop browser chrome, especially max-height around 720-820px
- narrow mobile preview if the deck supports it
- low viewport height

If browser tools are available, verify with screenshots or DOM snapshots.

## Viewport-Fit Gate

Fail the audit if any slide requires the user to zoom out or shrink the browser
to see full content.

Required checks:

- At 100% zoom, every slide's bottom content and footer/progress are visible.
- At 1280x720, no title/body/card/table/diagram overlaps or clips.
- At low desktop heights, a compact mode exists or the layout is already short enough.
- `grid-template-rows` should reserve flexible middle space: `auto minmax(0, 1fr) auto`.
- Dense components should have compact variants: reduced padding, row height, card height, and heading scale.
- If a table/list has too many rows for a slide, split it across slides instead of relying on smaller browser zoom.

Common fixes:

- reduce slide padding through `clamp()` and low-height media queries
- lower hero heading max size
- reduce card `min-height`
- shorten dense row copy
- convert a tall one-column stack into two columns
- split overloaded content into another slide

## Required Report Shape

When reviewing, lead with findings:

| Severity | Slide/File | Issue | Fix |
| --- | --- | --- | --- |

Then summarize:

- changed files
- remaining risk
- tests or browser checks run

If no issues are found, say so clearly and mention what was checked.

## Default Post-Generation Workflow

1. Read the generated HTML.
2. Identify slide count and navigation model.
3. Audit hierarchy, density, typography, color, and responsiveness.
4. Patch obvious issues directly when safe.
5. Re-run syntax checks and browser checks when available.
6. Hand off to motion polish.
