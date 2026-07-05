
## EDA Findings

### Class Imbalance (2026-07-05)
- Enterprise (CIC-IDS2017): 83.5% Benign / 16.5% Attack — realistic imbalance, ~2.15M benign rows available for training.
- IoT (CIC-IoT2023, xxsmall sample): 3.5% Benign / 96.5% Attack — inverted imbalance due to dense attack campaign design in data collection, not representative of real-world IoT traffic ratios. Still ~17,780 benign rows available for training. To be discussed as a dataset limitation in the manuscript.
# Daily Progress Log — ZeroDayML-Paper1

A lightweight, chronological log of day-to-day research activity. This complements `experiment_log.md` (formal experiment tracking) and `research_notes.md` (conceptual/methodological notes) by capturing granular task-level progress, blockers, and next actions — useful for PhD supervisory meetings and progress reports.

---

## How to Use This File

Add a new entry at the top (reverse chronological order) for each working session. Keep entries short and factual.

---

## Entry Template

```markdown
### YYYY-MM-DD

**Time spent:** <e.g., 3h>

**Worked on:**
-

**Completed:**
-

**Blockers / Issues:**
-

**Next steps:**
-

**Phase:** <Phase N — Name, per docs/project_plan.md>
```

---

## Log

### YYYY-MM-DD (Project Initialization)

**Time spent:** —

**Worked on:**
- Created initial repository scaffold (`ZeroDayML-Paper1`) following Cookiecutter Data Science structure.
- Drafted README, LICENSE, `.gitignore`, `requirements.txt`, `environment.yml`, `pyproject.toml`.
- Drafted full 8-phase project roadmap in `docs/project_plan.md`.

**Completed:**
- Repository scaffold ready for initial commit and push to GitHub.

**Blockers / Issues:**
- None yet — project in planning stage.

**Next steps:**
- Begin Phase 1: survey and select candidate enterprise + IoT datasets.
- Set up development environment (venv/conda) and validate `requirements.txt` / `environment.yml` install cleanly.

**Phase:** Phase 1 — Dataset Collection (starting)

---

*(Continue adding entries above this line as work progresses — most recent entry at the top is also acceptable; pick one convention and stay consistent.)*


### 2026-07-05

**Time spent:** ~2h

**Worked on:**
- Completed Phase 1: Downloaded CIC-IDS2017 (enterprise, 8 files, ~843MB) and CIC-IoT2023 sample (IoT, xxsmall, 508K rows) via Kaggle API in Google Colab.
- Completed Phase 2: Cleaned both datasets — fixed column name whitespace, fixed label encoding issue, removed missing/infinite values (Flow Bytes/s, Flow Packets/s), removed 255,236 duplicate rows, dropped redundant/non-feature columns.
- Finalized zero-day holdout strategy: attack-category holdout (not day-based), documented in research_notes.md.

**Completed:**
- Cleaned enterprise dataset: 2,572,640 rows x 78 columns, saved to data_interim/enterprise_cicids2017_cleaned.csv
- Cleaned IoT dataset: 508,155 rows x 47 columns, saved to data_interim/iot_ciciot2023_cleaned.csv

**Blockers / Issues:**
- CIC official server no longer serves direct downloads (redirects to request form) — used Kaggle mirrors instead.
- Using IoT "xxsmall" sample (508K rows) rather than full 13GB dataset for initial pipeline development; may scale up later.

**Next steps:**
- Begin Phase 3: Exploratory Data Analysis (EDA) on both cleaned datasets.

**Phase:** Phase 2 — Data Cleaning (complete) → Phase 3 starting
