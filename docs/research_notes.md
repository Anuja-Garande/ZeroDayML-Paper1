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
