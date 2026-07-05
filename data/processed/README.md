# data/processed/

**Purpose:** Final, model-ready datasets produced after feature engineering (Phase 4), including train/validation/test splits designed for the zero-day evaluation protocol.

## Rules

- These are the **only datasets that should be fed directly into model training/evaluation code** (`src/models/`, `scripts/train_*.py`).
- Must be reproducible from `data/interim/` via `src/features/` pipelines.
- Excluded from version control via `.gitignore` (only this README is tracked). Consider tracking dataset generation configs/hashes instead of raw files for reproducibility.

## Expected Contents

- `train.parquet` / `train.csv` — normal-only training data.
- `val.parquet` — validation data (normal + known attack types, for threshold tuning).
- `test_zero_day.parquet` — held-out test set containing attack classes unseen during training (the core zero-day evaluation set).
- `feature_metadata.json` — feature names, scaling parameters, and preprocessing provenance.

## Regeneration

```bash
python scripts/run_feature_engineering.py --config config/feature_config.yaml
```
