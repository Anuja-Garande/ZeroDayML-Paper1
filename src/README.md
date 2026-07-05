# src/

**Purpose:** Installable Python package containing all reusable, production-quality source code for the project. This is the single source of truth for reproducible pipeline logic (as opposed to exploratory notebooks).

## Subpackages

| Subpackage | Responsibility |
|------------|-----------------|
| `data/` | Data ingestion, cleaning, splitting (Phase 1–2) |
| `features/` | Feature engineering and transformation pipelines (Phase 4) |
| `models/` | Model definitions, training loops, and inference logic (Phase 5–6) |
| `evaluation/` | Metrics computation, statistical testing, evaluation protocols (Phase 7) |
| `visualization/` | Reusable plotting utilities for EDA and results reporting |
| `utils/` | Shared helpers: config loading, logging setup, random seeding, path management |

## Import Convention

After installing the package (`pip install -e .`), modules should be imported as:

```python
from src.data.make_dataset import load_raw_data
from src.features.build_features import build_behavioral_features
from src.models.isolation_forest_model import train_isolation_forest
from src.evaluation.metrics import compute_zero_day_metrics
from src.visualization.plots import plot_reconstruction_error
from src.utils.config import load_config
```

## Coding Standards

- Follow PEP 8; formatting enforced via `black` and `isort` (see `pyproject.toml`).
- Every public function/class must have a docstring (NumPy or Google style).
- No hardcoded file paths — always read from `config/` via `src/utils/config.py`.
- All randomness must be seeded via a shared utility (`src/utils/seed.py`) for reproducibility.
- Unit tests for every module live in `tests/`, mirroring this package's structure.
