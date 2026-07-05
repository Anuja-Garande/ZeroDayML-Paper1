# data/external/

**Purpose:** Third-party or auxiliary reference data that supports the project but is not a primary training/evaluation dataset.

## Examples of Expected Contents

- Threat intelligence feeds / known IOC (indicator of compromise) reference lists used for contextual analysis.
- Port/protocol reference tables (e.g., IANA service name mappings) used during feature engineering.
- Pretrained embeddings or auxiliary lookup tables (if used).

## Rules

- Document the source and license of every external file added here.
- Excluded from version control via `.gitignore` (only this README is tracked) unless a file is small and license-permits redistribution.
