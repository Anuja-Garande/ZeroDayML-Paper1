# Experiment Log — ZeroDayML-Paper1

This document tracks every formal experiment run for this project, ensuring full reproducibility and traceability from hypothesis → configuration → result. Every entry should correspond to a specific, reproducible run (ideally tied to a config file in `config/` and a git commit hash).

---

## How to Log an Experiment

Copy the template below for every new experiment. Keep entries append-only (do not delete old entries, even for failed runs — negative results are valuable for the dissertation).

---

## Experiment Log Template

```markdown
## EXP-<NNN>: <Short Descriptive Title>

- **Date:** YYYY-MM-DD
- **Author:** <Name>
- **Git Commit:** <commit-hash>
- **Config File:** config/<filename>.yaml
- **Dataset(s) Used:** <e.g., CIC-IDS2017 (enterprise), BoT-IoT (IoT)>
- **Model:** <e.g., Isolation Forest / One-Class SVM / LOF / Autoencoder>

### Hypothesis
What are you testing and why?

### Setup
- Train/validation/test split strategy:
- Zero-day held-out attack classes:
- Preprocessing / feature set version:
- Hyperparameters:

### Results

| Metric | Value |
|--------|-------|
| Precision | |
| Recall | |
| F1-score | |
| AUC-ROC | |
| AUC-PR | |
| False Positive Rate | |

### Artifacts
- Model checkpoint: `models/<filename>`
- Figures: `reports/figures/<filename>`
- Result table: `reports/tables/<filename>`

### Observations / Interpretation


### Next Steps

---
```

---

## Experiment Index

| ID | Date | Model | Dataset | Summary | Status |
|----|------|-------|---------|---------|--------|
| EXP-001 | _TBD_ | _TBD_ | _TBD_ | _Baseline Isolation Forest first run_ | Planned |
| EXP-002 | _TBD_ | _TBD_ | _TBD_ | _Baseline One-Class SVM first run_ | Planned |
| EXP-003 | _TBD_ | _TBD_ | _TBD_ | _Baseline LOF first run_ | Planned |
| EXP-004 | _TBD_ | _TBD_ | _TBD_ | _Autoencoder v1 architecture_ | Planned |

*(Add rows as experiments are executed. Keep this table in sync with the detailed entries above.)*
