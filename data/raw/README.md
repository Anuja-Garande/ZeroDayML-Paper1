# data/raw/

**Purpose:** Immutable, original, unmodified datasets as obtained from their source.

## Rules

- **Never edit files in this folder by hand or via scripts.** Treat this directory as read-only/immutable.
- Any transformation must write output to `data/interim/` or `data/processed/`, never back into `data/raw/`.
- Raw data files are **excluded from version control** via `.gitignore` due to size and licensing restrictions. Only this README and dataset documentation are tracked.

## Expected Contents (populated during Phase 1 — Dataset Collection)

| Dataset | Domain | Source / URL | Version / Date Downloaded | Checksum (SHA256) |
|---------|--------|---------------|----------------------------|--------------------|
| _TBD_ | Enterprise | _TBD_ | _TBD_ | _TBD_ |
| _TBD_ | IoT | _TBD_ | _TBD_ | _TBD_ |

## Download Instructions

Document exact download steps (or provide `scripts/download_data.py`) here once datasets are finalized, so any collaborator or reviewer can reproduce the raw data acquisition step exactly.

## Licensing Notes

Record any usage restrictions, citation requirements, or redistribution limitations for each dataset here.
