## Phase 7 — Final Consolidated Evaluation Summary (2026-07-05)

### Headline Results

- **Deep Autoencoder wins/ties on 10 of 14 attack groups (71%)** across both domains — the strongest single model overall.
- **Overall average AUC-ROC by model:** Autoencoder 0.829 > LOF 0.774 > One-Class SVM 0.763 > Isolation Forest 0.746
- **Overall average AUC-ROC by domain:** IoT 0.827 > Enterprise 0.713

### Win Distribution
- Autoencoder: 10 wins (across both domains)
- One-Class SVM: 3 wins
- LOF: 1 win
- Isolation Forest: 0 wins

### Key Narrative for Manuscript

1. **RQ1 (Can unsupervised learning trained only on normal behavior detect zero-day attacks?):** Yes, with substantial variation by attack type. DoS/DDoS-family and severe/rare attacks (Heartbleed, Infiltration, Mirai botnet) are detected reliably by ALL models (AUC-ROC > 0.88). Subtler behavioral attacks (scanning, brute force, botnet in enterprise; reconnaissance, brute force in IoT) are much harder, with enterprise scanning even falling below random guessing for Isolation Forest (0.470).

2. **RQ2 (Enterprise vs. IoT):** IoT zero-day detection is empirically easier than enterprise across ALL models tested (avg 0.827 vs 0.713), which contradicts the Phase 3 EDA-based hypothesis (UMAP suggested IoT would be harder due to visual overlap). This is an important methodological finding: 2D non-linear visualization can understate true separability present in the full high-dimensional feature space.

3. **RQ3 (Which features matter most?):** Addressed via EDA (Phase 3) — flow duration, throughput, and packet size show the strongest behavioral divergence for enterprise DoS/DDoS attacks; IoT shows inverted patterns (shorter duration, higher inter-arrival time for attacks). Formal feature importance/SHAP analysis remains a candidate for future work.

4. **RQ4 (Autoencoder vs. classical baselines):** The Deep Autoencoder achieves the best overall AUC-ROC (0.829 vs 0.746-0.774 for baselines), directly supporting the core thesis that non-linear representation learning improves zero-day generalization. However, a key nuanced finding is that AUC-ROC improvements do not always translate to usable F1/Recall at a fixed threshold (especially for enterprise scanning/brute_force/botnet), highlighting threshold selection as a critical, non-trivial component of any operational deployment.

### Deliverables
- `MASTER_results_all_models_all_domains.csv` — complete 56-row table (4 models × 14 attack groups) with all metrics
- `summary_statistics.json` — win counts, averages by domain/model
- `enterprise_all_models_comparison.png`, `iot_all_models_comparison.png` — headline comparison figures


# Research Notes — ZeroDayML-Paper1

This document is a living record of research decisions, literature insights, design rationale, and open questions encountered during the project. Unlike `experiment_log.md` (which tracks specific experiment runs) and `daily_progress.md` (which tracks day-to-day task progress), this file captures **conceptual and methodological reasoning**.

---

## How to Use This File

- Add a new dated entry for each significant research decision, insight, or literature finding.
- Cross-reference relevant papers stored in `references/`.
- Use this file when writing the Related Work and Methodology sections of the manuscript in Phase 8.

---

## Template for New Entries

```markdown
### YYYY-MM-DD — <Short Title>

**Context:**


**Insight / Decision:**


**Rationale:**


**References:**
-

**Impact on Project:**

```

---

## Literature Review Notes

### Key Themes to Cover

- Signature-based vs. behavior-based IDS approaches.
- Definitions and taxonomies of "zero-day" in the intrusion detection literature.
- Unsupervised anomaly detection methods applied to network security (Isolation Forest, One-Class SVM, LOF, clustering-based methods).
- Deep learning for anomaly detection (autoencoders, VAEs, GAN-based approaches).
- Enterprise vs. IoT network traffic characteristics and why models may not transfer directly between domains.
- Evaluation methodology critiques in IDS literature (e.g., data leakage issues, unrealistic train/test splits, dataset staleness).
- Explainable AI (XAI) in cybersecurity — why interpretability matters for SOC analyst trust and adoption.

### Datasets Reviewed

| Dataset | Domain | Year | Notes |
|---------|--------|------|-------|
| NSL-KDD | Enterprise | 2009 | Historical benchmark; known limitations (e.g., synthetic, dated attack types) |
| UNSW-NB15 | Enterprise | 2015 | Modern attack categories, widely used |
| CIC-IDS2017 / CSE-CIC-IDS2018 | Enterprise | 2017/2018 | Realistic traffic, commonly used in recent IDS literature |
| BoT-IoT | IoT | 2018 | IoT botnet traffic |
| TON_IoT | IoT/Enterprise (heterogeneous) | 2020 | Telemetry + network + OS data |
| CIC-IoT2023 | IoT | 2023 | Recent large-scale IoT attack dataset |

*(Expand this table as dataset selection is finalized in Phase 1.)*

---

## Methodological Design Decisions

### Zero-Day Simulation Protocol

Document here the exact strategy used to simulate zero-day conditions (e.g., which attack categories are withheld entirely from training and validation, and only introduced at test time).

### Feature Leakage Prevention

Document any features excluded due to leakage risk (e.g., IP addresses, exact timestamps, dataset-specific artifacts that trivially separate classes).

### Evaluation Protocol Justification

Document why chosen metrics (Precision, Recall, F1, AUC-ROC, AUC-PR, FPR) are appropriate given expected class imbalance and operational SOC constraints (e.g., false positive cost).

---

## Open Questions / Risks

- [ ] How will extreme class imbalance in zero-day evaluation sets be handled statistically?
- [ ] Will enterprise and IoT datasets be modeled separately or in a unified feature space?
- [ ] What is the appropriate anomaly threshold selection strategy for the autoencoder (fixed percentile vs. adaptive)?
- [ ] How will dataset-specific biases (e.g., synthetic traffic generation artifacts) be addressed/discussed as threats to validity?




## Zero-Day Holdout Strategy (Finalized)

**Decision:** Zero-day evaluation is designed around **attack category holdout**, not temporal (day-based) holdout. This directly tests whether models trained purely on normal behavior can detect previously unseen attack types, rather than just testing on a different day's traffic.

**Training set:** `BENIGN` traffic only, pooled across all 8 days of CIC-IDS2017 (~2.8M rows).

**Zero-day test groups (CIC-IDS2017):**

| Group | Attack Types | Approx. Rows |
|-------|-------------|--------------|
| DoS/DDoS family | DoS Hulk, DoS GoldenEye, DoS slowloris, DoS Slowhttptest, DDoS | ~375,000 |
| Scanning/Recon | PortScan | ~159,000 |
| Brute Force | FTP-Patator, SSH-Patator, Web Attack Brute Force | ~15,300 |
| Web-based | XSS, SQL Injection | ~673 |
| Botnet | Bot | ~1,966 |
| Rare/severe (exploratory only, small sample) | Heartbleed, Infiltration | 47 |

Each group is evaluated independently against the trained model, allowing per-attack-family performance breakdown rather than a single aggregate score. This supports a richer discussion in the manuscript (e.g., "the model detects volumetric DoS/DDoS well but struggles with low-signal attacks like Infiltration").

**Rationale:** Attack-category holdout is a stronger test of genuine zero-day generalization than day-based holdout, since attack types are not confounded with which day they were captured.
