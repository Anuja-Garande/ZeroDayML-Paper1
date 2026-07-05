# config/

**Purpose:** Centralized configuration files (YAML/JSON) that parameterize every stage of the pipeline, so experiments are reproducible without editing source code.

## Planned Configuration Files

| File | Purpose |
|------|---------|
| `data_config.yaml` | Raw data paths, cleaning parameters, dataset selection |
| `feature_config.yaml` | Feature engineering options, scaling method, train/val/test split & zero-day holdout definition |
| `baseline_config.yaml` | Hyperparameters for Isolation Forest, One-Class SVM, LOF |
| `autoencoder_config.yaml` | Autoencoder architecture, training hyperparameters, threshold strategy |
| `eval_config.yaml` | Evaluation metrics selection, reporting options |
| `logging_config.yaml` | Logging verbosity/format shared across scripts |

## Conventions

- All configs are version-controlled (they contain no secrets or credentials).
- Any file containing secrets/credentials (e.g., API keys) must be named with `*secret*` or `*local*` and is excluded via `.gitignore`.
- Each experiment run should reference an explicit config file path (never rely on hardcoded defaults) to preserve reproducibility, and the config filename/version should be recorded in `docs/experiment_log.md`.
