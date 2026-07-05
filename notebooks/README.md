# notebooks/

**Purpose:** Jupyter notebooks for exploration, visualization, and illustrative analysis. Notebooks are for **exploration and communication**, not for production pipeline logic — reusable logic should be refactored into `src/` modules and imported into notebooks.

## Naming Convention

Follow the Cookiecutter Data Science convention:

```
<phase-number>-<initials>-<short-description>.ipynb
```

Example:
```
01-jd-eda-enterprise-traffic.ipynb
02-jd-eda-iot-traffic.ipynb
03-jd-feature-engineering-exploration.ipynb
04-jd-baseline-model-prototyping.ipynb
05-jd-autoencoder-architecture-experiments.ipynb
06-jd-results-visualization.ipynb
```

## Guidelines

- Clear all cell outputs before committing large notebooks (or use `nbstripout`) to keep git history clean, unless the output figures are the point of the commit.
- Notebooks should import functions from `src/` rather than duplicating logic (`from src.features.build_features import ...`).
- Treat notebooks as disposable/exploratory — once an approach is validated, migrate it into `src/` and `scripts/` for reproducible execution.
- Register the project kernel before running:
  ```bash
  python -m ipykernel install --user --name zerodayml-paper1 --display-name "ZeroDayML-Paper1"
  ```
