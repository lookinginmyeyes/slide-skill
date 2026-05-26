---
name: slide-visual-assets
description: >-
  Plan, generate, and route visual assets for Hanbao single-file
  HTML slide decks, including host-native generated images, Raster Engine
  generated images, evidence images, product mockups, diagrams, infographics,
  architecture visuals, and placeholders. Use as a high-priority default for
  Hanbao slide generation whenever the environment supports image generation, Raster Engine,
  or host image generation and the user has not explicitly opted out.
  This skill directly calls the available image generation capability when the
  environment provides one. If image generation is unavailable, it returns a
  no-image or HTML-visual fallback decision instead of delivering prompts to the
  user.
---

# Slide Visual Assets

This skill decides what images or visuals a deck needs, prepares asset briefs,
and can call the environment's image generation capability to create actual
bitmap assets when available. In host or similar hosts with native image
generation, call the host image tool directly. In local API environments, call
Raster Engine through the retained scripts. It does not own deck layout. It hands
generated image paths, aspect ratios, fallback decisions, and placement notes to the HTML
slide generator.

This is a **mandatory max-power pre-generation asset production step** whenever
image generation / host image generation is available. Run it after
the deck plan, style/case direction, and typography are known, but before
writing the final HTML, unless the user has explicitly said not to use generated
images. Its job is to supply concrete image files for slides that benefit from
them and to assign different images different responsibilities.
Prompts are internal API inputs only, not a deliverable.

## API Configuration Boundary

Do not store API keys, base URLs, model-provider config, or gateway settings in
this skill package. Do not read `.env`, `.gateway.env`, or home-directory config
files from inside this module. Image generation must connect through:

- host image generation exposed by the current environment, or
- runtime environment variables injected by Hanbao/host.

If neither is available, return a fallback decision. Do not ask the user to
paste API secrets into the skill and do not use bundled gateway config.

This module keeps the fuller `raster-engine` prompt library and scripts, but the
Hanbao rules in this file are the adapter. Use retained templates only after the
slide asset role is clear.

## Output Contract

For each relevant slide, produce:

```text
Slide:
Asset role:
Asset type:
Aspect ratio:
Generation mode:
Generated file path:
Placement note:
No-image fallback:
```

When producing multiple assets, return one record per asset. Each asset must
have a unique slug and output path so generation can run concurrently.

Asset roles:

- evidence
- product context
- concept anchor
- process / diagram
- atmosphere
- cover / chapter opener
- UI mock / screenshot
- data illustration

Do not request images for every slide. But when image generation is available,
do not stop at one generic hero image. For a normal polished Hanbao deck, plan a
small system of assets with distinct responsibilities, typically:

- cover / chapter anchor
- product or use-case scene
- evidence or context visual
- architecture / process / diagram visual
- UI mock / screenshot-style state
- data illustration or conceptual object when it clarifies the story

Skip a role only when it would be irrelevant or decorative.

## Mode Detection

Use the strongest available mode. If image generation, Raster Engine, or host-native image
generation is available, attempt real image generation by default for all
meaningful deck visuals unless the user explicitly opts out. Do not wait for the
user to ask for images. Do not satisfy this module by producing only prompts or
one generic image.

1. **Mode A · Host-host image generation**: if the host has a native image
   generation tool or image API, call it directly with the final prompt. In
   host, use the host image generation capability exposed by the host.
2. **Mode B · Local Raster Engine generation**: if a local image generation
   API/tool is configured, call Raster Engine using the retained scripts and save
   images to disk.
3. **Mode C · No-image fallback**: if no image tool is available, do not return
   prompts as the output. Return `Generated file path: none` plus a concrete
   no-image fallback such as HTML/CSS/SVG diagram, data card, product mock panel,
   typographic visual, or "no image needed".

Deliver generated image files, placement intent, and fallback decisions. A
generation request by itself is not a user-facing deliverable.

Full retained mode and script guidance is retained at:

- `references/raster-engine/full-reference.md`
- `scripts/raster-engine/scripts/check-mode.js`
- `scripts/raster-engine/scripts/generate.js`
- `scripts/raster-engine/scripts/edit.js`

Use those scripts when the environment is configured for local Raster Engine
generation and no host-native image tool is available. Otherwise, use the
templates to shape the internal API request and pass the final prompt to the
host image tool.

## Raster Engine Generation Workflow

When actual image generation is needed:

1. Read this file and assign asset responsibilities across the deck.
2. Pick one relevant generation template per asset family from
   `references/raster-engine/references/`.
3. Render an internal image-generation request with explicit purpose, subject, composition, style,
   text policy, aspect ratio, and negative constraints.
4. Check whether the host environment exposes a host image generation
   capability. If yes, call it directly with the final prompt, save or embed the
   returned image according to the host's file model, record the generated file
   path, and skip the local script path.
5. If no host-native image tool is available, run the local Raster Engine mode
   detector:

```bash
node slide-visual-assets/scripts/raster-engine/scripts/check-mode.js --json
```

6. If local Raster Engine generation is available, call:

```bash
node slide-visual-assets/scripts/raster-engine/scripts/generate.js \
  --promptfile <prompt-file> \
  --size 1536x864 \
  --quality high \
  --output <deck-assets-dir>/<asset-slug>.png
```

7. Save generated images under the current deck's asset directory, for example:

```text
<deck-slug>-assets/
  image/
    cover-hero.png
    architecture-visual.png
  prompt/
    cover-hero.md
    architecture-visual.md
```

8. Pass the generated file path to `hanbao-html-slide`.
9. For final single-file HTML delivery, either reference the local image path
   when Hanbao can package it, or embed the image as base64 when the deck must be
   fully self-contained.

If real generation fails due to network/API/auth issues, report the failure and
fall back to the no-image path. Do not claim image generation succeeded and do
not output prompts as a replacement.

## Parallel Image Generation

After the deck plan is known, batch assets by independence:

- cover and chapter openers
- product/UI scenes
- evidence images
- architecture/process/diagram visuals
- infographic or data-illustration tiles

Generate independent assets concurrently when the host/tooling supports it. Use
one prompt file and one output image path per asset. Do not reuse the same output
path across concurrent calls.

Recommended concurrency:

- Host-native image tool: follow host limits; prefer 2-4 concurrent requests when allowed.
- Local Raster Engine scripts: 2-3 concurrent processes by default.
- If the API rate-limits or returns transient network errors, retry the failed asset once, then fall back for that asset only.

While images are generating, the main slide generator may continue with:

- selected template/resource preparation
- typography token mapping
- slide copy refinement
- motion grammar planning

Do not block the whole deck on a non-essential decorative image. Block only for
assets that are structurally required by the selected layout.

Recommended slide asset sizes:

- 16:9 hero / section image: `1536x864`
- 4:3 evidence / product visual: `1344x1008`
- 1:1 icon/object/diagram tile: `1024x1024`

## When To Run

Run after the deck plan and selected visual direction are known, and before
`hanbao-html-slide` writes final HTML.

Run at max power by default when:

- the environment supports image generation / host image generation,
- the user has not explicitly refused generated images,
- the selected style is editorial, magazine, product-demo, launch, pitch, or
  visually rich,
- a slide needs concrete evidence or visual explanation,
- the deck contains abstract concepts that need anchors,
- the source includes images, screenshots, charts, or diagrams.

Skip when:

- the deck is intentionally text-only,
- the user says not to use generated images,
- the artifact must avoid external or generated assets.

## Asset Selection Rules

- Evidence beats decoration.
- For business claims, prefer screenshots, charts, customer scenes, or concrete
  objects over abstract gradients.
- For technical content, prefer architecture diagrams, annotated flows, or UI
  state diagrams.
- For narrative/editorial decks, use fewer, stronger images.
- For data slides, do not replace charts with decorative images.
- For product decks, image assets should reveal actual state, workflow, or
  product value.
- If the asset is only atmosphere, use it mainly on cover/chapter slides.
- Each generated asset needs a named responsibility. Reject vague roles such as
  "nice background", "AI visual", or "decorative illustration" unless the slide
  is explicitly a mood-setting chapter opener.

## Internal Generation Request Shape

Use structured prompts internally when calling an image generator:

```text
Purpose:
Subject:
Composition:
Style:
Lighting / color:
Text policy:
Output constraints:
Negative constraints:
```

Default text policy: generated images should contain no readable text unless the
user explicitly asks for text in the image. Add labels in HTML instead.

## Prompt Strengthening

Before calling an image generator, strengthen each prompt internally:

- state the slide job the image serves;
- specify subject, composition, visual hierarchy, material/lighting, crop, and
  safe negative constraints;
- match the selected deck style and typography without embedding text;
- reserve negative space for HTML copy when the slide layout needs it;
- forbid stock-photo blandness, generic AI gradients, malformed UI text,
  meaningless icons, and decorative clutter;
- include aspect ratio and how the asset will be cropped or embedded.

Never show these prompts to the user as content. The deliverable is the
generated image or a concrete no-image HTML/SVG fallback.

## Image Brief Families

### Editorial Cover

Use for chapter openers, launch story, market narrative.

Relevant retained templates:

- `references/raster-engine/references/poster-and-campaigns/editorial-cover.md`
- `references/raster-engine/references/poster-and-campaigns/banner-hero.md`
- `references/raster-engine/references/poster-and-campaigns/campaign-kv.md`

Emphasize composition, materiality, atmosphere, and room for HTML typography.

### Product / UI Scene

Use for SaaS, workflow, dashboard, product demo.

Relevant retained templates:

- `references/raster-engine/references/ui-mockups/landing-page-case-study.md`
- `references/raster-engine/references/ui-mockups/chat-interface-scene.md`
- `references/raster-engine/references/ui-mockups/product-card-overlay.md`
- `references/raster-engine/references/product-visuals/premium-studio-product.md`
- `references/raster-engine/references/product-visuals/lifestyle-product-scene.md`

Show a believable interface state, but keep fine text illegible or omitted so
the real deck can overlay precise labels.

### Architecture / System Visual

Use for technical explanation.

Relevant retained templates:

- `references/raster-engine/references/technical-diagrams/system-architecture.md`
- `references/raster-engine/references/technical-diagrams/flowchart-decision.md`
- `references/raster-engine/references/technical-diagrams/sequence-diagram.md`
- `references/raster-engine/references/technical-diagrams/network-topology.md`
- `references/raster-engine/references/academic-figures/method-pipeline-overview.md`

Prefer clean isometric or schematic visuals with distinct nodes and flows. Add
labels in HTML, not inside the generated image.

### Infographic Object

Use when a slide needs a metaphorical but structured visual.

Relevant retained templates:

- `references/raster-engine/references/infographics/bento-grid-infographic.md`
- `references/raster-engine/references/infographics/comparison-infographic.md`
- `references/raster-engine/references/infographics/kpi-dashboard-infographic.md`
- `references/raster-engine/references/slides-and-visual-docs/visual-report-page.md`
- `references/raster-engine/references/slides-and-visual-docs/educational-diagram-slide.md`

Avoid generic icons. Use one strong object or diagrammatic scene.

## Integration With HTML Decks

- Keep final deck single-file compatible.
- Use generated host-native or Raster Engine outputs as local files or base64-embedded assets
  according to the environment.
- If an image cannot be generated or embedded, use the no-image fallback from
  this module. Do not include hidden or visible prompts as a substitute.
- Avoid external hotlinks unless the environment explicitly permits them.
- When the deck is generated inside Hanbao, prefer passing generated image paths
  through the deck asset manifest or converting them to base64 during final HTML
  assembly.

## Self-Check

- Does every requested image serve a slide job?
- If image generation was requested, did the skill actually attempt host-native
  image generation first, then local Raster Engine if needed, before falling back?
- Are generated file paths recorded and passed to the HTML generator?
- If generation failed, did the module return a concrete HTML visual/no-image
  fallback instead of a prompt?
- Is the internal generation request specific enough to generate a usable composition?
- Is there enough negative guidance to avoid stock-like filler?
- Is text handled in HTML instead of inside the image?
- Does the aspect ratio match the target layout?

## Reference Routing

| Need | Read |
| --- | --- |
| Internal image request writing | `references/raster-engine/references/prompt-writing.md` |
| Slide/document visuals | `references/raster-engine/references/slides-and-visual-docs/` specific template |
| Editorial cover/KV | `references/raster-engine/references/poster-and-campaigns/` specific template |
| Product/UI mock | `references/raster-engine/references/ui-mockups/` or `product-visuals/` specific template |
| Technical architecture/flow | `references/raster-engine/references/technical-diagrams/` specific template |
| Academic figure | `references/raster-engine/references/academic-figures/` specific template |
| Infographic | `references/raster-engine/references/infographics/` specific template |
| Image editing | `references/raster-engine/references/editing-workflows/` specific template |

Do not load all 90+ templates. Pick the family from the slide asset role, then
read only one or two templates.
