---
name: slide-source-grounding
description: >-
  Extract, ground, and summarize source material for Hanbao slide decks from
  local folders, markdown/text, PDF, DOCX, XLSX/CSV, or mixed knowledge-base
  materials. Use before narrative planning when a deck should be based on
  documents, evidence, reports, tables, references, or knowledge-base content.
  This skill distills progressive retrieval and source-aware extraction without
  owning slide rendering.
---

# Slide Source Grounding

This skill prepares reliable source material for slide generation. It does not
write the final deck. It produces a source-backed evidence pack for
`slide-narrative-planner` and `hanbao-html-slide`.

This module keeps fuller retained `source-reader` guidance and scripts, but the
Hanbao adapter in this file decides what evidence the slide system needs.

## Output Contract

Return a compact evidence pack:

```text
Source scope:
Files inspected:
Key evidence:
1. Claim/evidence:
   Source:
   Location:
   Confidence:
   Slide use:

Useful quotes:
Data points:
Open gaps:
```

Do not fabricate citations or data. If a claim is inferred, mark it as inferred.

## When To Run

Run when:

- the user provides files or a folder,
- the deck must be based on reports, PDFs, spreadsheets, docs, or a knowledge
  base,
- the user asks for citations, evidence, references, data, or "based on this",
- the source is too long to read fully into the prompt.

Skip when:

- the user provides a short, self-contained brief,
- the deck is purely conceptual or visual exploration,
- the user asks only to restyle an existing deck.

## Retrieval Workflow

1. Identify the source scope.
2. Inspect directory/index files first if present.
3. Choose the most relevant candidate files.
4. Search locally with targeted keywords before reading large files.
5. For each hit, read only the nearby context.
6. Extract claims, numbers, quotes, dates, names, and source locations.
7. Stop when there is enough evidence for the requested deck.

Use `rg` for text search where possible.

## File-Type Rules

### Markdown / Text

- Search by keywords.
- Read local sections around matches.
- Preserve headings as source locations.

### PDF

- Use a PDF extraction method before searching.
- Keep page numbers or approximate page locations.
- For tables, extract structure when possible instead of copying visual text.

For detailed PDF handling, read
`references/source-reader/references/pdf_reading.md`. The retained helper script
`scripts/source-reader/convert_pdf_to_images.py` is available when page images
are needed for visual inspection.

### DOCX

- Extract text and headings before summarizing.
- Preserve section names when possible.

### XLSX / CSV

- Inspect sheets, columns, first rows, and data types.
- Filter or aggregate targeted data instead of reading the whole workbook.
- Keep units, date ranges, and column names.

For detailed spreadsheet handling, read:

- `references/source-reader/references/excel_reading.md`
- `references/source-reader/references/excel_analysis.md`

## Evidence Quality

Prefer:

1. measured data,
2. direct source statements,
3. user-provided product facts,
4. dated reports,
5. clearly marked assumptions.

Avoid:

- unsupported claims,
- isolated numbers without units,
- context-free quotes,
- over-reading a single source.

## Integration With Deck Planning

Pass evidence to `slide-narrative-planner` in slide-ready chunks:

- claim
- supporting evidence
- source location
- recommended visual type
- possible asset need

Do not force all evidence into the main deck. Mark appendix candidates.

## Self-Check

- Are the most important claims source-backed?
- Are numbers accompanied by units and dates?
- Are source locations specific enough to verify?
- Are assumptions clearly marked?
- Is the evidence pack compact enough for the planner to use without rereading
  everything?

## Reference Routing

| Need | Read |
| --- | --- |
| Full retained retrieval workflow | `references/source-reader/full-reference.md` |
| PDF extraction | `references/source-reader/references/pdf_reading.md` |
| Excel reading | `references/source-reader/references/excel_reading.md` |
| Excel analysis | `references/source-reader/references/excel_analysis.md` |
| Mixed source extraction pattern | `references/extraction-patterns.md` |

Do not read every source file or every retained reference by default. Start from
the user's source scope and retrieve progressively.
