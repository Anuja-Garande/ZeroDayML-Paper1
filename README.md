# ZeroDayML-Paper1

**Behavior-Based Zero-Day Intrusion Detection using Unsupervised Machine Learning for Enterprise and IoT Networks**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Status](https://img.shields.io/badge/status-active--research-orange.svg)]()

---

## 1. Project Overview

`ZeroDayML-Paper1` is the first research artifact in a PhD research program investigating **unsupervised and semi-supervised machine learning approaches for detecting zero-day (previously unseen) intrusions** based on network and host **behavioral patterns**, applicable across both **enterprise IT networks** and **resource-constrained IoT networks**.

Unlike signature-based intrusion detection systems (IDS), which fail against novel or previously unseen attacks, this project focuses on **behavior-based anomaly detection**, where models learn a representation of "normal" network/device behavior and flag statistically or structurally significant deviations as candidate zero-day intrusions.

This repository is designed to be:

- **Reproducible** — every experiment, dataset transformation, and result is traceable and re-runnable.
- **Publication-ready** — structured to directly support a peer-reviewed journal submission.
- **Extensible** — designed as the foundation ("Paper 1") for a multi-paper PhD dissertation trajectory.

---

## 2. Research Objective

**Primary Objective:**
To design, implement, and evaluate unsupervised machine learning models capable of detecting zero-day intrusions in enterprise and IoT network traffic by modeling normal behavioral baselines, without relying on labeled attack signatures.

**Specific Objectives:**

1. Curate and preprocess benchmark and behavior-rich network traffic datasets (enterprise + IoT).
2. Perform exploratory data analysis (EDA) to characterize normal vs. anomalous behavioral patterns.
3. Engineer behavior-based statistical, temporal, and flow-level features.
4. Establish baseline unsupervised anomaly detection models:
   - Isolation Forest
   - One-Class SVM
   - Local Outlier Factor (LOF)
5. Design and train a **Deep Autoencoder** for unsupervised anomaly detection as the primary proposed model.
6. Rigorously evaluate all models using standard IDS evaluation metrics (Precision, Recall, F1, AUC-ROC, AUC-PR, False Positive Rate) under zero-day (unseen-attack) evaluation protocols.
7. Interpret model decisions using explainability techniques (SHAP, LIME).
8. Document findings and prepare the manuscript for journal submission.

**Research Questions:**

- RQ1: Can unsupervised learning models trained only on "normal" behavior generalize to detect previously unseen (zero-day) attack classes?
- RQ2: How does model performance differ between enterprise network traffic and IoT network traffic?
- RQ3: What behavioral features contribute most to distinguishing zero-day intrusions from benign traffic?
- RQ4: How does a Deep Autoencoder compare against classical unsupervised baselines (Isolation Forest, One-Class SVM, LOF) in zero-day detection settings?

---

## 3. Repository Structure

```
ZeroDayML-Paper1/
│
├── README.md                  <- This file
├── LICENSE                    <- MIT License
├── .gitignore                 <- Ignore rules for Python/ML/IDE/OS artifacts
├── requirements.txt           <- Pip dependency list
├── environment.yml            <- Conda environment specification
├── pyproject.toml             <- Project metadata, tooling, and build config
│
├── config/                    <- YAML/JSON configuration files (paths, hyperparameters, dataset configs)
│
├── data/
│   ├── raw/                   <- Immutable original datasets (never modified)
│   ├── interim/                <- Intermediate, partially cleaned data
│   ├── processed/              <- Final, model-ready datasets
│   └── external/                <- Third-party / auxiliary reference datasets
│
├── notebooks/                 <- Jupyter notebooks for exploration (naming: `01-eda-network-traffic.ipynb`)
│
├── src/                       <- Source code package for use in this project
│   ├── data/                    <- Scripts to download, load, and split data
│   ├── features/                <- Feature engineering and transformation code
│   ├── models/                  <- Model definitions, training, and inference code
│   ├── evaluation/              <- Metrics, evaluation protocols, statistical testing
│   ├── visualization/           <- Plotting and reporting utilities
│   ├── utils/                   <- Shared helpers (logging, config parsing, seeding)
│   └── __init__.py
│
├── models/                    <- Serialized trained models, checkpoints, scalers
│
├── reports/
│   ├── figures/                 <- Generated graphics/plots for the paper
│   ├── tables/                  <- Generated result tables (CSV/LaTeX)
│   └── paper/                   <- Manuscript drafts, LaTeX/Word source
│
├── docs/                       <- Project documentation, roadmap, research notes, logs
│
├── tests/                      <- Unit and integration tests (pytest)
│
├── scripts/                    <- Standalone CLI scripts (e.g., run_pipeline.py, train.py)
│
└── references/                 <- Papers, datasets documentation, related literature (PDF/BibTeX)
```

This layout follows the **Cookiecutter Data Science** philosophy: a clean separation between raw data, code, models, and outputs, ensuring the research pipeline is auditable and reproducible from raw data to published results.

---

## 4. Installation Instructions

### Option A: Using `venv` + `pip`

```bash
git clone https://github.com/<your-username>/ZeroDayML-Paper1.git
cd ZeroDayML-Paper1

python3 -m venv .venv
source .venv/bin/activate        # On Windows: .venv\Scripts\activate

pip install --upgrade pip
pip install -r requirements.txt
```

### Option B: Using Conda

```bash
git clone https://github.com/<your-username>/ZeroDayML-Paper1.git
cd ZeroDayML-Paper1

conda env create -f environment.yml
conda activate zerodayml-paper1
```

### Option C: Using `pyproject.toml` (editable install)

```bash
pip install -e .
```

### Register Jupyter Kernel

```bash
python -m ipykernel install --user --name zerodayml-paper1 --display-name "ZeroDayML-Paper1"
```

---

## 5. How to Run

> **Note:** This repository currently contains the **project scaffold only**. Machine learning code, notebooks, and pipelines will be added incrementally as the research progresses (see `docs/project_plan.md`).

Once implemented, the intended usage pattern will be:

```bash
# 1. Place raw datasets into data/raw/ (see data/raw/README.md)

# 2. Run the data preparation pipeline
python scripts/run_data_pipeline.py --config config/data_config.yaml

# 3. Run feature engineering
python scripts/run_feature_engineering.py --config config/feature_config.yaml

# 4. Train baseline models (Isolation Forest, One-Class SVM, LOF)
python scripts/train_baselines.py --config config/baseline_config.yaml

# 5. Train the Deep Autoencoder model
python scripts/train_autoencoder.py --config config/autoencoder_config.yaml

# 6. Run evaluation and generate reports/figures
python scripts/evaluate_models.py --config config/eval_config.yaml
```

Notebooks under `notebooks/` will mirror this pipeline for exploratory and illustrative purposes but are **not** the source of truth for reproducible results — `src/` and `scripts/` are.

---

## 6. Planned Workflow

The project follows an 8-phase roadmap, fully detailed in [`docs/project_plan.md`](docs/project_plan.md):

| Phase | Description |
|-------|-------------|
| Phase 1 | Dataset Collection (enterprise + IoT network traffic) |
| Phase 2 | Data Cleaning & Preprocessing |
| Phase 3 | Exploratory Data Analysis (EDA) |
| Phase 4 | Behavior-Based Feature Engineering |
| Phase 5 | Baseline Unsupervised Models (Isolation Forest, One-Class SVM, LOF) |
| Phase 6 | Deep Autoencoder Design & Training |
| Phase 7 | Evaluation under Zero-Day Protocols |
| Phase 8 | Journal Paper Writing & Submission |

Progress is tracked in [`docs/daily_progress.md`](docs/daily_progress.md) and [`docs/experiment_log.md`](docs/experiment_log.md).

---

## 7. Citation

If you use this repository, methodology, or results in your own research, please cite:

```bibtex
@article{ZeroDayMLPaper1,
  author  = {<Your Full Name>},
  title   = {Behavior-Based Zero-Day Intrusion Detection using Unsupervised Machine Learning for Enterprise and IoT Networks},
  journal = {<Target Journal Name>},
  year    = {2026},
  note    = {Manuscript in preparation},
  url     = {https://github.com/<your-username>/ZeroDayML-Paper1}
}
```

A formal citation (with DOI) will be added upon publication.

---

## 8. License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## Author / Maintainer

**PhD Researcher:** `<Your Name>`
**Affiliation:** `<Your University / Department>`
**Contact:** `<your-email@domain.com>`

This repository is part of an ongoing PhD dissertation research program on machine learning for cybersecurity. Future companion repositories (`ZeroDayML-Paper2`, `ZeroDayML-Paper3`, etc.) will extend this work.
