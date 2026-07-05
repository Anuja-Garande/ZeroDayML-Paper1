# models/

**Purpose:** Serialized trained model artifacts — checkpoints, fitted scalers/encoders, and exported model files.

## Rules

- Large binary model files (`*.h5`, `*.pkl`, `*.joblib`, `*.pt`, `*.onnx`, etc.) are **excluded from version control** via `.gitignore` due to size.
- For reproducibility, every model artifact saved here must be traceable to:
  1. The exact config file used (`config/*.yaml`)
  2. The git commit hash of the code that produced it
  3. An entry in `docs/experiment_log.md`
- Consider using descriptive, versioned filenames, e.g.:
  ```
  isolation_forest_v1_2026-01-15.joblib
  autoencoder_v3_enterprise_2026-02-10.h5
  scaler_standard_v1.joblib
  ```

## Recommended Practice

For large models or datasets exceeding GitHub's size limits, consider:
- [Git LFS](https://git-lfs.github.com/) for versioned large-file storage, or
- External storage (institutional server, Zenodo, OSF) with download links documented here for reproducibility.
