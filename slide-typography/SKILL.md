---
name: slide-typography
description: >-
  Apply or design typography presets for single-file HTML slide decks. Use when
  the user wants to choose, compare, extract, or improve fonts, type scale,
  heading/body rhythm, Chinese-English mixed typography, mono labels, or a
  premium Editorial Luxe type style. This skill only controls typography tokens
  and rules; it does not generate a whole deck by itself.
---

# Slide Typography

Typography preset skill for HTML slide decks.

This skill controls type, not the whole deck. It should produce or modify typography tokens that another slide generator applies.

Typography selection is mandatory before final deck generation. Do not silently choose a default preset. If the user has not already picked one, ask them to choose from the presets below.

## Output Contract

- Produce typography tokens, CSS variables, and usage rules.
- Keep the target compatible with single-file HTML decks.
- Do not create a multi-file font pipeline.
- Do not require npm, build steps, Vite, React, or a dev server.
- External webfont links are allowed only when the deployment environment permits them. Always provide strong fallbacks.

Bundled references:

- `references/typography.md`
- `references/typeset.md`

Read them when the task is specifically about improving type, line-height, font pairing, hierarchy, or detailed typesetting.

Do not read typography references by default if the user simply chooses a preset. Apply the preset tokens directly. Read references only when:

- the preset fails in a concrete deck,
- the user asks for a custom typography system,
- mixed Chinese/English readability needs deeper adjustment,
- or visual audit flags typography as a problem.

## Selection Prompt

When a deck is about to be generated and the user has not selected typography, ask:

```text
最后选一下字体气质：
1. Hanbao Default：干净无衬线，适合产品、销售、内部汇报
2. Editorial Luxe：高对比衬线标题 + 斜体强调，适合高级 B2B 叙事
3. Swiss Light：轻字重无衬线，适合数据、高管汇报、技术说明
4. Editorial Serif：克制衬线标题，适合品牌故事、行业观察、杂志感演示
```

Do not describe `Editorial Luxe` using the huashu name. It is a distilled typography option, not a user-facing source label.

## Presets

### Hanbao Default

Use for balanced product, sales, and internal decks.

```css
--font-display: "Avenir Next", "PingFang SC", "Noto Sans SC", system-ui, sans-serif;
--font-body: "Avenir Next", "PingFang SC", "Noto Sans SC", system-ui, sans-serif;
--font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;

--h1-size: clamp(52px, 6.4vw, 104px);
--h1-weight: 820;
--h1-line: .94;
--h2-size: clamp(36px, 4.2vw, 68px);
--h2-weight: 760;
--h2-line: 1;
--body-size: clamp(17px, 1.35vw, 23px);
--body-line: 1.42;
--meta-size: 11px;
--meta-weight: 800;
--meta-tracking: .12em;
```

### Editorial Luxe

Use for premium B2B editorial decks with high-contrast serif headlines, italic emphasis words, and polished commercial storytelling. This preset distills the reference typography feel without using the huashu name.

Best for:

- B2B sales or prospecting decks
- premium SaaS/product narratives
- founder pitch moments
- brand-led commercial storytelling
- landing-page-like opening slides

```css
--font-display: Georgia, "Times New Roman", "Songti SC", "Noto Serif SC", serif;
--font-emphasis: Georgia, "Times New Roman", "Songti SC", "Noto Serif SC", serif;
--font-body: "Avenir Next", "PingFang SC", "Noto Sans SC", system-ui, sans-serif;
--font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;

--h1-size: clamp(58px, 7.2vw, 124px);
--h1-weight: 500;
--h1-line: .9;
--h1-tracking: 0;
--emphasis-style: italic;
--emphasis-weight: 520;
--h2-size: clamp(38px, 4.7vw, 76px);
--h2-weight: 500;
--h2-line: .96;
--body-size: clamp(18px, 1.45vw, 24px);
--body-line: 1.52;
--body-weight: 400;
--meta-size: 12px;
--meta-weight: 760;
--meta-tracking: .12em;
```

Rules:

- Use serif display type for large headlines and key emotional phrases.
- Use italic emphasis for one short phrase or word, not whole paragraphs.
- Keep body copy in a clean sans-serif so the deck remains readable.
- Pair with compact gold/amber labels or refined accent color when appropriate.
- Use mono only for metadata, tags, counters, file names, or small system labels.
- Do not over-tighten Chinese text or force italic behavior where Chinese fallback looks poor.

### Swiss Light

Use for strict grid, executive, technical, or data-heavy decks.

```css
--font-display: "Inter", "Helvetica Neue", "PingFang SC", "Noto Sans SC", sans-serif;
--font-body: "Inter", "Helvetica Neue", "PingFang SC", "Noto Sans SC", sans-serif;
--font-mono: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, monospace;

--h1-size: clamp(54px, 6.8vw, 112px);
--h1-weight: 240;
--h1-line: .9;
--h2-size: clamp(34px, 4.4vw, 72px);
--h2-weight: 300;
--h2-line: .96;
--body-size: clamp(16px, 1.25vw, 22px);
--body-line: 1.38;
--meta-size: 11px;
--meta-weight: 600;
--meta-tracking: .16em;
```

Rules:

- Use alignment and scale, not heavy weight, to create hierarchy.
- Prefer hairlines and whitespace over cards.
- Avoid bold hero titles unless the content is intentionally loud.

### Editorial Serif

Use for story-led, magazine, founder narrative, industry observation, and brand decks.

```css
--font-display: Georgia, "Times New Roman", "Songti SC", "Noto Serif SC", serif;
--font-body: "Avenir Next", "PingFang SC", "Noto Sans SC", system-ui, sans-serif;
--font-mono: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;

--h1-size: clamp(52px, 6.6vw, 108px);
--h1-weight: 520;
--h1-line: .94;
--h2-size: clamp(34px, 4.5vw, 72px);
--h2-weight: 520;
--h2-line: 1;
--body-size: clamp(17px, 1.35vw, 23px);
--body-line: 1.48;
--meta-size: 11px;
--meta-weight: 800;
--meta-tracking: .13em;
```

Rules:

- Use serif for display, not for dense UI panels.
- Give headings enough room.
- Pair with strong image rhythm or editorial whitespace.

## Selection Guidance

Use this only when the user asks for advice. Do not silently pick a preset from this guidance.

- Product/SaaS/workflow: Hanbao Default or Editorial Luxe.
- Design-forward product story: Editorial Luxe.
- Executive/data/technical: Swiss Light.
- Narrative/brand/industry story: Editorial Serif.

## Audit Checklist

Before returning typography changes:

- Heading/body contrast is at least 1.25x by size or weight.
- Body line length stays readable, roughly 45-75 characters where possible.
- Chinese and English line-height both feel comfortable.
- Meta labels do not overpower main content.
- Font fallback does not destroy hierarchy.
- Buttons, tags, and counters do not resize layout unexpectedly.
