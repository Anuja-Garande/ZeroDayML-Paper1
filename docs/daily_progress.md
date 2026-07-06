## EXP-004, EXP-005, EXP-006: Baseline Models — IoT Dataset

- **Date:** 2026-07-05
- **Models:** Isolation Forest, One-Class SVM, LOF (same hyperparameters as enterprise experiments)
- **Dataset:** CIC-IoT2023 (xxsmall sample)
- **Training:** 14,400 benign rows (full set, no subsampling needed)

### Results Summary (AUC-ROC by attack group, best model bolded)

| Attack Group | Isolation Forest | One-Class SVM | LOF |
|---|---|---|---|
| ddos | 0.883 | 0.903 | **0.953** |
| dos | 0.899 | 0.960 | **0.979** |
| mirai_botnet | 0.972 | 0.988 | **0.998** |
| reconnaissance | 0.683 | 0.695 | **0.753** |
| spoofing_mitm | 0.768 | 0.792 | **0.797** |
| brute_force | 0.584 | 0.687 | **0.764** |
| web_based | 0.683 | **0.775** | 0.753 |
| other_malware | 0.699 | **0.837** | 0.794 |

**Key Finding 1 — Contradicts Phase 3 EDA Prediction:** UMAP visualization (Phase 3) showed heavy benign/attack overlap for IoT, leading to a prediction that baselines would underperform on IoT vs. enterprise. This was NOT confirmed — all IoT baseline results are substantially stronger and more consistent than enterprise results, with every attack group achieving AUC-ROC > 0.68 for at least one model (compare to enterprise, where 2 of 6 groups fell below or near random guessing). This suggests 2D non-linear projections (UMAP) can understate true separability that exists in the full high-dimensional feature space, an important methodological caveat for the paper.

**Key Finding 2 — Cross-Domain Consistency:** LOF is again the most frequent best-performing model (6 of 8 IoT groups; 3 of 6 enterprise groups), reinforcing that local density-based anomaly detection is particularly well-suited to behavior-based zero-day detection across both enterprise and IoT domains.

**Key Finding 3 — Mirai/Botnet near-perfect detection:** All three models achieve AUC-ROC > 0.97 on mirai_botnet, likely because Mirai-based attacks produce highly distinctive, repetitive flooding behavior very unlike normal IoT device telemetry.
## EXP-003: Local Outlier Factor Baseline — Enterprise Dataset

- **Date:** 2026-07-05
- **Model:** LOF (n_neighbors=20, contamination='auto', novelty=True)
- **Dataset:** CIC-IDS2017 (enterprise)
- **Training:** Same 20,000 row benign subsample as One-Class SVM

### Results by Zero-Day Attack Group

| Attack Group | Precision | Recall | F1 | AUC-ROC | AUC-PR | FPR |
|---|---|---|---|---|---|---|
| dos_ddos | 0.856 | 0.938 | 0.895 | 0.934 | 0.882 | 0.157 |
| scanning | 0.743 | 0.458 | 0.567 | 0.705 | 0.671 | 0.159 |
| brute_force | 0.665 | 0.308 | 0.421 | 0.661 | 0.709 | 0.156 |
| web_based | 0.331 | 0.085 | 0.135 | 0.310 | 0.395 | 0.171 |
| botnet | 0.708 | 0.387 | 0.500 | 0.663 | 0.594 | 0.160 |
| rare_severe | 0.750 | 0.638 | 0.690 | 0.776 | 0.777 | 0.213 |

## Baseline Comparison Summary (All 3 Models, AUC-ROC)

| Attack Group | Isolation Forest | One-Class SVM | LOF | Best |
|---|---|---|---|---|
| dos_ddos | 0.896 | 0.895 | 0.934 | LOF |
| scanning | 0.470 | 0.537 | 0.705 | LOF |
| brute_force | 0.738 | 0.439 | 0.661 | Isolation Forest |
| web_based | 0.653 | 0.664 | 0.310 | One-Class SVM |
| botnet | 0.556 | 0.556 | 0.663 | LOF |
| rare_severe | 0.967 | 0.960 | 0.776 | Isolation Forest |

**Key Findings:**
1. No single baseline dominates across all attack types — model choice matters significantly depending on attack behavior.
2. LOF (local density-based) substantially outperforms global methods (Isolation Forest, One-Class SVM) on scanning and botnet attacks, suggesting these attacks form locally-distinguishable but globally-overlapping patterns.
3. LOF consistently shows higher false positive rates (15.6-21.3%) than Isolation Forest/OC-SVM (4.3-8.7%), reflecting a precision/recall tradeoff — LOF catches more attacks but raises more false alarms.
4. Isolation Forest performs best on rare_severe (Heartbleed/Infiltration) and competitively on brute_force, suggesting global isolation is well-suited to attacks with genuinely extreme statistical outlier behavior.
5. web_based attacks remain difficult for all three baselines (best AUC-ROC only 0.664), representing the hardest attack category in this evaluation — a candidate for the Deep Autoencoder to potentially improve upon in Phase 6.

**Conclusion:** These baseline results establish a strong empirical foundation for comparison. No classical unsupervised method provides consistently reliable zero-day detection across all attack families, motivating the Deep Autoencoder as a potentially more robust, non-linear alternative in Phase 6.
## EXP-002: One-Class SVM Baseline — Enterprise Dataset

- **Date:** 2026-07-05
- **Model:** One-Class SVM (kernel='rbf', nu=0.05, gamma='scale')
- **Dataset:** CIC-IDS2017 (enterprise)
- **Training:** 20,000 row subsample of benign data (full dataset infeasible due to O(n²-n³) SVM scaling)

### Results by Zero-Day Attack Group

| Attack Group | Precision | Recall | F1 | AUC-ROC | AUC-PR | FPR |
|---|---|---|---|---|---|---|
| dos_ddos | 0.939 | 0.797 | 0.862 | 0.895 | 0.923 | 0.052 |
| scanning | 0.226 | 0.015 | 0.029 | 0.537 | 0.465 | 0.053 |
| brute_force | 0.024 | 0.001 | 0.002 | 0.439 | 0.455 | 0.049 |
| web_based | 0.377 | 0.030 | 0.055 | 0.664 | 0.543 | 0.049 |
| botnet | 0.398 | 0.034 | 0.062 | 0.556 | 0.496 | 0.051 |
| rare_severe | 0.952 | 0.851 | 0.899 | 0.960 | 0.966 | 0.043 |

### Cross-Model Comparison (Isolation Forest vs. One-Class SVM)

Both models show a nearly identical performance pattern across attack types despite using entirely different algorithmic approaches (tree-based isolation vs. kernel-based boundary estimation):
- **Strong on:** dos_ddos, rare_severe (both AUC-ROC > 0.89)
- **Weak on:** scanning, brute_force, web_based, botnet (AUC-ROC 0.44-0.66)

**Key Interpretation:** This cross-algorithm consistency suggests the difficulty is rooted in the underlying data/feature representation, not a weakness specific to either algorithm. DoS/DDoS and severe/rare attacks produce genuinely distinct flow-level behavioral signatures; scanning/brute-force/web/botnet attacks overlap substantially with benign traffic in the current 77-feature space. This motivates the need for a more expressive model (Deep Autoencoder) capable of learning non-linear feature interactions, per Phase 6.




## EXP-001: Isolation Forest Baseline — Enterprise Dataset

- **Date:** 2026-07-05
- **Model:** Isolation Forest (n_estimators=100, contamination='auto')
- **Dataset:** CIC-IDS2017 (enterprise)
- **Training:** 1,717,519 benign rows only

### Results by Zero-Day Attack Group

| Attack Group | Precision | Recall | F1 | AUC-ROC | AUC-PR | FPR |
|---|---|---|---|---|---|---|
| dos_ddos | 0.900 | 0.778 | 0.835 | 0.896 | 0.871 | 0.087 |
| scanning | 0.032 | 0.003 | 0.005 | 0.470 | 0.443 | 0.086 |
| brute_force | 0.082 | 0.007 | 0.013 | 0.738 | 0.622 | 0.080 |
| web_based | 0.265 | 0.027 | 0.049 | 0.653 | 0.608 | 0.074 |
| botnet | 0.228 | 0.023 | 0.041 | 0.556 | 0.524 | 0.076 |
| rare_severe | 0.977 | 0.894 | 0.933 | 0.967 | 0.975 | 0.021 |

### Key Finding: Anomaly Score Direction Analysis

Investigated why scanning/brute_force/web_based/botnet performed poorly by comparing mean anomaly scores (benign vs. attack):

| Group | Benign Score | Attack Score | Difference |
|---|---|---|---|
| scanning | 0.374 | 0.340 | -0.034 (inverted) |
| botnet | 0.374 | 0.355 | -0.019 (inverted) |
| brute_force | 0.373 | 0.399 | +0.026 (correct, weak) |
| web_based | 0.373 | 0.378 | +0.005 (correct, negligible) |

**Interpretation:** DoS/DDoS and rare/severe attacks (Heartbleed, Infiltration) are strongly and correctly detected by Isolation Forest — these attacks produce flow statistics (duration, throughput) genuinely distinct from normal traffic. However, scanning and botnet attacks are systematically ranked as MORE normal than actual benign traffic by the isolation mechanism — likely because their short, simple, repetitive flow patterns are trivially "isolated" in ways the algorithm associates with typical behavior. Brute force and web-based attacks show correct-direction but very weak separation, suggesting the signal exists but requires a more expressive model (e.g., deep autoencoder) to exploit reliably.

**Implication:** Isolation Forest is NOT a reliable general-purpose zero-day detector across all attack families — performance is highly attack-type-dependent. This motivates comparing against One-Class SVM and LOF (different inductive biases) and ultimately the Deep Autoencoder as the primary proposed method.


## Phase 4 — Feature Engineering (Completed 2026-07-05)

### Zero-Day Test Groups Finalized

**Enterprise (CIC-IDS2017):** 6 groups — dos_ddos (321,759), scanning (90,694), brute_force (10,620), web_based (673), botnet (1,948), rare_severe (47)

**IoT (CIC-IoT2023):** 8 groups — ddos (216,000), dos (72,000), mirai_botnet (54,000), reconnaissance (74,262), spoofing_mitm (36,000), brute_force (13,064), web_based (21,611), other_malware (3,218)

### Train/Validation Split
- Enterprise: 1,717,519 train / 429,380 validation (benign only, 80/20 split)
- IoT: 14,400 train / 3,600 validation (benign only, 80/20 split) — small benign pool due to dataset's inverted class imbalance noted in EDA

### Scaling
- StandardScaler fit exclusively on benign training data for each domain (no leakage from attack or validation data)
- Enterprise: 77


### Dimensionality Reduction — IoT Dataset (2026-07-05)

- PCA (2 components) captured only 28.2% of variance — similar limitation to enterprise data.
- UMAP showed HEAVY overlap between benign and attack points throughout the embedding space, in sharp contrast to the enterprise dataset's more distinguishable attack clusters. Only a few small regions show partial separation.

**Key comparative finding (Enterprise vs. IoT):** Attack traffic in the IoT dataset is far less visually separable from benign traffic than in the enterprise dataset, even under non-linear dimensionality reduction. This suggests IoT attack detection is a substantially harder problem at the individual-flow level — likely because benign IoT device traffic is already bursty/repetitive by nature, reducing the contrast with flood-style attacks compared to enterprise traffic.

**Implication for RQ2 and methodology:** This finding directly motivates the use of a Deep Autoencoder over simpler linear/distance-based methods — if strong non-linear visualization (UMAP) still shows substantial class overlap in 2D, the true separating signal likely requires learning complex combinations across the full feature space, which shallow 2D projections cannot capture. This also predicts that baseline models (Isolation Forest, One-Class SVM, LOF) may underperform on IoT data relative to enterprise data, an empirical question to test in Phase 5-7.


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
