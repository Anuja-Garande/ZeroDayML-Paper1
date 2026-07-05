# Project Plan & Roadmap — ZeroDayML-Paper1

**Project:** Behavior-Based Zero-Day Intrusion Detection using Unsupervised Machine Learning for Enterprise and IoT Networks
**Type:** PhD Research — Paper 1 of dissertation series
**Status:** Planning / Scaffold Stage

---

## Overview

This document defines the complete phased roadmap for `ZeroDayML-Paper1`. Each phase has defined objectives, deliverables, and exit criteria. Progress against this plan should be tracked in `docs/daily_progress.md`, and individual experiments should be logged in `docs/experiment_log.md`.

---

## Phase 1 — Dataset Collection

**Objective:** Identify, acquire, and document benchmark datasets representative of both enterprise and IoT network behavior.

**Tasks:**
- Survey candidate datasets:
  - Enterprise: CIC-IDS2017, CSE-CIC-IDS2018, UNSW-NB15, NSL-KDD
  - IoT: CIC-IoT2023, BoT-IoT, TON_IoT, N-BaIoT
- Document licensing, availability, and citation requirements for each dataset in `references/`.
- Download raw datasets into `data/raw/` (excluded from version control; documented via `data/raw/README.md`).
- Record dataset schema, feature definitions, and class distributions.
- Define the "zero-day simulation protocol": which attack classes will be withheld from training to simulate unseen/zero-day attacks during evaluation.

**Deliverables:**
- `data/raw/README.md` describing dataset sources, versions, and checksums.
- `references/dataset_notes.md` (or `.bib` entries) with citations.

**Exit Criteria:** At least one enterprise dataset and one IoT dataset acquired, documented, and validated for integrity.

---

## Phase 2 — Data Cleaning

**Objective:** Produce a clean, consistent, model-ready dataset from raw sources.

**Tasks:**
- Handle missing values, duplicate records, and corrupted rows.
- Normalize inconsistent column names/types across datasets.
- Resolve class imbalance considerations (documented, not yet corrected — correction happens in Phase 4/5 pipelines as needed).
- Encode categorical/protocol fields appropriately.
- Remove/flag non-behavioral identifier fields (e.g., raw IP addresses) to prevent leakage.
- Version cleaned datasets into `data/interim/`.

**Deliverables:**
- Data cleaning scripts under `src/data/`.
- Cleaning summary report in `reports/tables/`.

**Exit Criteria:** Reproducible cleaning pipeline that transforms `data/raw/` → `data/interim/` with a documented, deterministic process.

---

## Phase 3 — Exploratory Data Analysis (EDA)

**Objective:** Build deep understanding of behavioral patterns, feature distributions, and separability between normal and attack traffic.

**Tasks:**
- Univariate and multivariate analysis of traffic features.
- Class distribution analysis (benign vs. attack categories).
- Correlation analysis and multicollinearity checks.
- Temporal pattern analysis (traffic behavior over time).
- Dimensionality reduction visualization (PCA, t-SNE, UMAP) to visually assess cluster separability.
- Missing data visualization (`missingno`).

**Deliverables:**
- EDA notebooks under `notebooks/` (e.g., `01-eda-enterprise.ipynb`, `02-eda-iot.ipynb`).
- Key figures exported to `reports/figures/`.
- EDA summary findings documented in `docs/research_notes.md`.

**Exit Criteria:** Documented behavioral insights that directly inform feature engineering decisions in Phase 4.

---

## Phase 4 — Feature Engineering

**Objective:** Engineer behavior-based features that capture network/host activity patterns relevant to zero-day anomaly detection.

**Tasks:**
- Flow-level statistical features (packet counts, byte counts, duration, inter-arrival times).
- Behavioral aggregation features (per-host, per-window activity summaries).
- Protocol-specific behavioral indicators.
- Feature scaling/normalization strategy (StandardScaler/MinMaxScaler) fit only on "normal" training data.
- Feature selection / dimensionality reduction (variance thresholding, mutual information, PCA/UMAP as needed).
- Train/validation/test split strategy explicitly designed for **zero-day evaluation** (i.e., certain attack types fully withheld from training).

**Deliverables:**
- Feature engineering pipeline in `src/features/`.
- Final model-ready datasets in `data/processed/`.
- Feature dictionary documented in `docs/research_notes.md`.

**Exit Criteria:** Finalized, versioned feature set with documented rationale, ready for baseline modeling.

---

## Phase 5 — Baseline Models

**Objective:** Establish classical unsupervised anomaly detection baselines trained only on normal behavior.

**Models:**
1. **Isolation Forest** — tree-based anomaly isolation scoring.
2. **One-Class SVM** — boundary-based novelty detection.
3. **Local Outlier Factor (LOF)** — density-based local anomaly detection.

**Tasks:**
- Implement consistent training/evaluation interface for all three models (`src/models/`).
- Hyperparameter search (contamination rate, kernel choice, `n_neighbors`, etc.) via validation set (normal-only + held-out known attacks, not zero-day attacks).
- Establish baseline performance metrics on the zero-day evaluation protocol.

**Deliverables:**
- Baseline model implementations and configs (`config/baseline_config.yaml`).
- Baseline results table in `reports/tables/`.

**Exit Criteria:** Reproducible baseline results for all three models, logged in `docs/experiment_log.md`.

---

## Phase 6 — Deep Autoencoder

**Objective:** Design, implement, and train a Deep Autoencoder as the primary proposed unsupervised anomaly detection model.

**Tasks:**
- Architecture design (encoder-decoder depth, bottleneck dimensionality, activation functions).
- Training strategy: train exclusively on normal/benign behavioral data; anomaly score = reconstruction error.
- Regularization strategy (dropout, batch normalization, early stopping) to prevent overfitting to training-set idiosyncrasies.
- Threshold selection strategy for anomaly classification (e.g., percentile-based reconstruction error threshold on validation set).
- Optional extensions: variational autoencoder (VAE), denoising autoencoder — documented as future work if not implemented in Paper 1.

**Deliverables:**
- Autoencoder implementation in `src/models/`.
- Trained model checkpoints in `models/`.
- Training curves and architecture diagrams in `reports/figures/`.

**Exit Criteria:** Trained autoencoder with documented architecture and training behavior, ready for comparative evaluation.

---

## Phase 7 — Evaluation

**Objective:** Rigorously and fairly compare all models (baselines + autoencoder) under the zero-day detection protocol.

**Tasks:**
- Compute standard metrics: Precision, Recall, F1-score, AUC-ROC, AUC-PR, False Positive Rate, Detection Rate.
- Evaluate specifically on **withheld/unseen attack classes** to directly answer the zero-day research questions.
- Statistical significance testing across model comparisons (e.g., paired tests across cross-validation folds).
- Model interpretability analysis using SHAP and LIME to explain feature contributions to anomaly scores.
- Comparative analysis: Enterprise vs. IoT network performance.
- Ablation studies (e.g., feature set variations, architecture variations) as time permits.

**Deliverables:**
- Evaluation pipeline in `src/evaluation/`.
- Final results tables (`reports/tables/`) and figures (`reports/figures/`).
- Interpretability plots (SHAP summary plots, LIME explanations).

**Exit Criteria:** Complete, statistically sound comparative evaluation directly answering RQ1–RQ4.

---

## Phase 8 — Journal Paper Writing

**Objective:** Translate research outputs into a submission-ready journal manuscript.

**Tasks:**
- Identify target journal(s) (e.g., IEEE Access, Computers & Security, Journal of Network and Computer Applications).
- Draft manuscript sections: Abstract, Introduction, Related Work, Methodology, Experimental Setup, Results, Discussion, Threats to Validity, Conclusion & Future Work.
- Prepare camera-ready figures/tables from `reports/`.
- Internal review, advisor review, and revision cycles.
- Prepare reproducibility statement and link to this repository.
- Submit, respond to reviewer comments, and finalize.

**Deliverables:**
- Manuscript draft(s) in `reports/paper/`.
- Final submitted/published version with citation added to `README.md`.

**Exit Criteria:** Manuscript submitted to target journal.

---

## Timeline Summary (indicative — adjust as needed)

| Phase | Description | Target Duration |
|-------|-------------|------------------|
| 1 | Dataset Collection | 2–3 weeks |
| 2 | Data Cleaning | 2 weeks |
| 3 | EDA | 2–3 weeks |
| 4 | Feature Engineering | 3 weeks |
| 5 | Baseline Models | 3 weeks |
| 6 | Deep Autoencoder | 4 weeks |
| 7 | Evaluation | 3–4 weeks |
| 8 | Journal Paper Writing | 4–6 weeks |

---

## Future PhD Expansion (Beyond Paper 1)

This repository is designed as the foundation for subsequent papers, potentially including:

- **Paper 2:** Semi-supervised / self-supervised extensions incorporating limited labeled attack data.
- **Paper 3:** Federated learning approaches for privacy-preserving distributed IoT intrusion detection.
- **Paper 4:** Real-time / streaming zero-day detection under concept drift.
- **Paper 5:** Adversarial robustness of unsupervised IDS models.

Each future paper is expected to follow the same reproducibility structure established here, likely as sibling repositories (`ZeroDayML-Paper2`, etc.) that may depend on shared utilities extracted from this repository's `src/utils/`.
