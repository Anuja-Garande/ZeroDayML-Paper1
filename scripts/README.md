# scripts/

**Purpose:** Standalone, CLI-executable entry-point scripts that orchestrate the `src/` package modules into complete pipeline stages. These are the primary reproducible entry points for running the full research pipeline end-to-end (as referenced in the main `README.md` → "How to Run").

## Planned Scripts

| Script | Purpose |
|--------|---------|
| `download_data.py` | Download/verify raw datasets into `data/raw/` |
| `run_data_pipeline.py` | Orchestrate data cleaning (`data/raw/` → `data/interim/`) |
| `run_feature_engineering.py` | Build final model-ready features (`data/interim/` → `data/processed/`) |
| `train_baselines.py` | Train Isolation Forest, One-Class SVM, and LOF baseline models |
| `train_autoencoder.py` | Train the Deep Autoencoder model |
| `evaluate_models.py` | Run full zero-day evaluation protocol across all trained models |
| `generate_report_figures.py` | Generate all figures/tables for `reports/` from saved results |

## Conventions

- Every script must accept a `--config` argument pointing to a YAML file in `config/`.
- Every script must set a deterministic random seed via `src.utils.seed`.
- Every script should log its run (parameters, timestamp, output paths) to support entries in `docs/experiment_log.md`.
- Scripts should contain minimal logic themselves — they should primarily call functions from `src/` to keep business logic testable and reusable.

## Example Usage (once implemented)

```bash
python scripts/run_data_pipeline.py --config config/data_config.yaml
python scripts/train_baselines.py --config config/baseline_config.yaml
```
