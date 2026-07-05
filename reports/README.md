# reports/

**Purpose:** All generated outputs intended for human consumption — figures, tables, and the manuscript itself.

## Structure

- `figures/` — Generated plots (PNG/SVG/PDF) used in EDA, results analysis, and the manuscript. Regeneratable from `src/visualization/` and notebooks; large binary figures are excluded from version control (see `.gitignore`), with key final figures for the paper committed explicitly if needed.
- `tables/` — Generated result tables (CSV, and LaTeX `.tex` tables) summarizing experiment outcomes, suitable for direct inclusion in the manuscript.
- `paper/` — Manuscript drafts (LaTeX or Word), submission-ready PDFs, cover letters, and reviewer response documents.

## Guidelines

- Every figure/table in `reports/paper/` should be traceable back to the exact script/notebook and experiment ID (`docs/experiment_log.md`) that generated it.
- Use consistent, publication-quality styling (see `src/visualization/` for shared plotting style configuration) across all figures.
- Keep raw editable sources (`.tex`, `.docx`) alongside exported `.pdf` versions in `reports/paper/`.
