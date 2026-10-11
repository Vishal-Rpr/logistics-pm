---
name: style-matching
description: "This skill matches the user's personal writing style when generating PM documents. It should be used alongside logistics-prd-generation or any document generation task. It reads reference documents from the reference-docs/ directory and instructs the generation process to replicate the user's structure, tone, level of detail, and formatting conventions. Use when the user says 'match my style', 'write like my other PRDs', 'use my format', or when generating any PRD, spec, or document where reference-docs/ contains example files."
---

# Style Matching from Reference Documents

## Purpose

Generate documents that sound like the user wrote them, not like a generic AI template. This skill reads the user's existing PRDs and specs from `reference-docs/` and extracts style patterns to replicate.

## Instructions

1. Read only these two style PRDs. They are the same files the RAG co-pilot uses. Do not use `sample-user-story.md` as PRD style.
   - `reference-docs/credit-limit-prd.md`
   - `reference-docs/rate-benchmarking-prd.md`
   If either file is missing, skip style matching and note it in the output.
2. Read those two documents. Extract the following style signals:

### Structure Signals
- What sections do they use? In what order?
- Do they use a component-based structure (separate sections per feature component) or a monolithic flow?
- How deep is the heading hierarchy? (H2 only? H2 + H3? H2 + H3 + H4?)
- Do they use horizontal rules (`---`) to separate major sections?

### Detail Level Signals
- How long is the Overview section? (1 sentence? 2-3 sentences? A paragraph?)
- Do they define data points in tables or in prose?
- Do they include calculated fields and formulas?
- How specific are their acceptance criteria?
- Do they include key considerations sections per component?

### Tone Signals
- Formal or conversational?
- Active voice or passive?
- Do they use "we" or "the system"?
- Do they use bold for emphasis or keep it minimal?
- Do they use jargon freely or define terms?

### Formatting Signals
- Tables for data definitions?
- Bullet points or numbered lists?
- Code blocks for formulas or system behavior?
- Consistent naming conventions for fields?

3. When generating a new document, explicitly replicate these signals. The goal is that the output, placed next to the reference docs, looks like it came from the same author.

## Example Style Extraction

From a reference PRD that uses:
- Component-based sections with H2 for components and H3 for subsections
- Data point tables with Field | Description | Source columns
- Calculated fields shown as bold formulas
- Key considerations as bullet lists per component
- Non-goals stated explicitly
- Open questions framed as genuine questions
- Success metrics with specific targets

The generated PRD should replicate all of these patterns, not default to a generic template.

## Fallback

If either style PRD named above is missing:
- Use the template from `skills/logistics-prd-generation/references/prd-template.md`
- Note in the output: "No reference style documents found. Using standard template. Add your existing PRDs to reference-docs/ for personalized style matching."

## Notes

- Style matching is about structure and voice, not content. The new PRD should cover its own topic fully while sounding like the user.
- If the reference docs vary in style (e.g., one is detailed, another is brief), default to the more detailed style.
- Do not copy content from reference docs. Only replicate the structural and tonal patterns.
