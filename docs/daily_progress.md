
### Dimensionality Reduction — Enterprise Dataset (2026-07-05)

- PCA (2 components) captured only 37.4% of variance, heavily skewed by outlier flows — linear projection is not sufficient to represent this data's structure.
- UMAP (non-linear) revealed clearer cluster structure: distinct attack-type clusters visible as separate streaks/groups, with benign traffic forming its own dense clusters. Partial overlap exists between some benign and attack points in central regions, suggesting certain attacks behave similarly enough to normal traffic to be genuinely challenging to detect.

**Implication:** Non-linear class separability supports the use of non-linear unsupervised models (e.g., deep autoencoder, Isolation Forest, LOF) over purely linear approaches (e.g., PCA-based anomaly scoring) for this detection task.


### Feature-Level Behavioral Differences — IoT Dataset (2026-07-05)

| Feature | Benign (median) | Attack (median) |
|---------|-----------------|------------------|
| flow_duration | 26.39 | 0.256 |
| Rate | 52.83 | 20.22 |
| Tot size (bytes) | 246.8 | 101.2 |
| IAT (inter-arrival time) | 0.053 | 83,254,140 |

**Interpretation:** IoT attack traffic shows the OPPOSITE pattern from enterprise attack traffic — shorter flow duration, lower packet rate, smaller packets, but vastly larger inter-arrival times. This is consistent with flood-style attacks (DDoS/DoS) generating many short, bursty connection attempts rather than sustained connections, while benign IoT device traffic (telemetry, regular check-ins) is steadier and longer-lived.

**Key implication:** Behavioral signatures of "attack" vs "normal" are domain-specific and not transferable between enterprise and IoT contexts — a feature pattern indicating attack in one domain can indicate normal behavior in the other. This strongly supports training and evaluating separate models per domain (enterprise vs. IoT) rather than a single unified model, and is a notable comparative finding for RQ2 in the manuscript.


### Feature-Level Behavioral Differences — Enterprise Dataset (2026-07-05)

| Feature | Benign (median) | Attack (median) | Ratio |
|---------|-----------------|------------------|-------|
| Flow Duration (µs) | 32,817 | 6,031,838 | Attack ~184x longer |
| Total Fwd Packets | 2 | 4 | Attack ~2x more packets |
| Flow Bytes/s | 4,529 | 143 | Benign ~32x more throughput |
| Fwd Packet Length Mean (bytes) | 38.0 | 8.7 | Benign ~4x larger packets |

**Interpretation:** Attack traffic in CIC-IDS2017 tends toward long-duration, low-throughput, small-packet behavior — consistent with sustained scans, brute-force attempts, and flood-style attacks that maximize connection time while minimizing payload per packet. Benign traffic shows short-duration, high-throughput, larger-payload patterns typical of normal web/file transfer activity. This supports the core premise that "behavior" (not just content) meaningfully separates normal from malicious traffic, motivating the flow-level statistical feature approach used throughout this project.


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
