# Paper to Academic PPT Skill

An installable Agent Skill for turning scientific papers and supplied
PowerPoint templates into editable academic presentations.

The workflow keeps a page-level evidence trail for every slide while producing:

- editable PowerPoint slides that inherit the supplied master and layout;
- source figures placed from page-indexed PDF assets;
- native editable tables and charts when backed by paper data;
- speaker notes that explain transitions, emphasis and likely questions;
- PDF and PNG render previews generated from the actual PPTX;
- evidence maps, validation results, uncertainties and change records.

## Start here

- [Skill instructions](SKILL.md)
- [Deck plan schema](references/deck-plan.schema.json)
- [Template and layout rules](references/template-and-layout.md)
- [Evidence and notes rules](references/evidence-and-notes.md)
- [Minimum example](references/examples.md)
- [Quality rubric](references/quality-rubric.md)
- [Self-check cases](references/self-check-cases.md)

## Dependency check

```bash
python -c "import pptx, pypdf, PIL; print(pptx.__version__, pypdf.__version__, PIL.__version__)"
```

Install declared dependencies with `python -m pip install -r requirements.txt`.
PDF-to-PNG rendering additionally requires Poppler `pdftoppm` or PyMuPDF.
OTF/TTF conversion requires LibreOffice `soffice` or Microsoft PowerPoint.

## Smoke test

```bash
python scripts/selftest.py
```

The test creates synthetic input, builds and renders a two-slide deck, verifies
embedded notes, and confirms that an invalid evidence quote is rejected.

## Scope

This repository contains only the reusable Skill and its public support files.
It contains no hidden-competition papers, prepared answers, benchmark scores, or
fabricated run logs.
