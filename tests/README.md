# tests/

**Purpose:** Unit and integration tests for the `src/` package, using `pytest`.

## Conventions

- Mirror the `src/` package structure, e.g.:
  ```
  tests/
  ├── data/
  │   └── test_make_dataset.py
  ├── features/
  │   └── test_build_features.py
  ├── models/
  │   └── test_isolation_forest_model.py
  ├── evaluation/
  │   └── test_metrics.py
  └── utils/
      └── test_config.py
  ```
- Test file names must start with `test_` and test functions with `test_` (per `pyproject.toml` pytest configuration).
- Use small synthetic fixtures rather than full datasets for fast, deterministic unit tests.
- Aim for meaningful coverage of data transformation logic, feature computation, and evaluation metric correctness — not just high coverage percentage.

## Running Tests

```bash
pytest
```

With coverage report (already configured in `pyproject.toml`):

```bash
pytest --cov=src --cov-report=term-missing
```
