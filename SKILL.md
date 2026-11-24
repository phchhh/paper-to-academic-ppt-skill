---
name: paper-to-academic-ppt
description: Convert scientific papers, PDFs, reports, and templates into editable academic PowerPoint decks with page-level evidence maps, speaker notes, and rendered previews. Use when a source paper must become a constrained slide narrative rather than a paragraph-by-paragraph summary.
---

# Paper to Academic PPT

Build a presentation that tells the paper's argument through a small number of
evidence-backed slides. The output must remain editable, fit the supplied
template, and contain real rendered previews.

## Runtime contract

Read `task.md` and `case.json` first. Resolve material paths relative to their
declaring JSON file, then the input directory. Use `/app/work` for intermediates
and `/app/output` for final artifacts. Preserve every field in
`case.output_contract`, including its exact name and nesting.

Do not invent paper results, sample sizes, figure numbers, citation details or
benchmark scores. Do not describe an unrendered deck as rendered. Formal cases
come from the platform and must not be embedded in this Skill.

## Workflow

1. Prepare page-aware source and template inventories:

   ```bash
   python scripts/academic_ppt.py prepare \
     --case /input/case.json --input /input --out /app/output
   ```

   Inspect `work/inventory.json`, `work/template_report.json` and
   `work/source_index.json`. The command also extracts PDF figure candidates to
   `work/assets/source/`.

2. Define the narrative from the actual task. Each slide gets one purpose and
   one main claim. Use the source to establish the problem, method, decisive
   evidence, interpretation, limitations and conclusion. Omit repetitive
   background before the core result.

3. Create `/app/work/deck_plan.json` using
   [references/deck-plan.schema.json](references/deck-plan.schema.json). Copy
   evidence quotes from the indexed text. Select only real extracted figures,
   and keep figure/table/panel IDs exactly as reported.

4. Generate and validate:

   ```bash
   python scripts/academic_ppt.py finalize \
     --case /input/case.json --plan /app/work/deck_plan.json \
     --out /app/output
   ```

   This writes an editable PPTX, page-level evidence map, `result.json`, and a
   render report. At least one real PDF page and PNG preview must exist for a
   `success` status.

5. Inspect every rendered page. Check title/body fit, clipping, overlap, image
   crop, figure numbering, units, source footer and template integrity. Correct
   the plan and rerun `finalize`.

6. If a template is supplied, stay inside its theme and placeholders. If an
   existing deck is supplied as the starting point, use its requested slides or
   layouts before adding slides. Read
   [references/template-and-layout.md](references/template-and-layout.md) for
   mapping rules.

## Slide standard

- One slide makes one point. A takeaway plus at most three concise support
  points is usually enough.
- Put the exact page/figure/table reference in the plan and visible footer where
  appropriate. Preserve panel letters, captions, units, error bars and
  comparator direction.
- Speaker notes explain transitions, emphasis and anticipated questions. Do not
  repeat slide prose verbatim.
- Keep figures and data tables as embedded images, native editable tables or
  native charts. Do not replace scientific evidence with decorative graphics.
- Do not use tiny text to rescue overloaded slides. Split the slide or remove
  secondary detail.
- Respect fixed slide count, audience, language and deck duration when present.

Read [references/evidence-and-notes.md](references/evidence-and-notes.md) for
page mapping and notes rules. Read
[references/examples.md](references/examples.md) for a minimal valid plan and
the expected output envelope. Use
[references/quality-rubric.md](references/quality-rubric.md) for the final pass.
Run `python scripts/selftest.py` for an isolated end-to-end check.

## Failure handling

- Missing template: use the neutral built-in layout and report that choice.
- Unreadable/scanned PDF: mark status `partial` unless OCR is available and
  verified; never infer figure content from filenames.
- Missing figure extraction: use the cited text/table evidence and record the
  missing visual.
- Placeholder shortage: use a compatible layout or a safe text region while
  preserving master header/footer rules; note the fallback.
- Font/render failure: retain the PPTX, set a non-success status and provide the
  exact renderer error.
- Template or output corrupted: do not claim success; report the failing file
  and validation code.

Keep revisions in `change_log`. A partial result must identify the missing page,
visual, template feature or render check.
