# data/interim/

**Purpose:** Intermediate, partially cleaned/transformed data produced from `data/raw/` during Phase 2 (Data Cleaning).

## Rules

- Files here are **derived and reproducible** — they should always be re-creatable by running the scripts in `src/data/` against `data/raw/`.
- Do not treat this folder as a permanent store; it can be safely deleted and regenerated.
- Excluded from version control via `.gitignore` (only this README is tracked).

## Expected Contents

- Deduplicated / missing-value-handled datasets.
- Schema-normalized datasets (consistent column names/types across sources).
- Intermediate merged enterprise + IoT datasets (if applicable).

## Regeneration

```bash
python scripts/run_data_pipeline.py --stage clean --config config/data_config.yaml
```
