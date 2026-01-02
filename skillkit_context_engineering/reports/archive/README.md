# Reports Archive

> **Purpose**: Store historical gate results and summaries.

## Directory Structure

```
archive/
├── 2026-01-02/
│   ├── gate_preflight.json
│   ├── gate_ci.json
│   ├── gate_semantic.json
│   └── summary.md
├── 2026-01-03/
│   └── ...
└── ...
```

## Archiving Procedure

After each successful run:

```bash
ARCHIVE_DIR="skillkit_context_engineering/reports/archive/$(date +%Y-%m-%d)"
mkdir -p $ARCHIVE_DIR
cp skillkit_context_engineering/reports/latest/* $ARCHIVE_DIR/
```

## Retention Policy

- Keep at least last 10 runs
- Keep all runs related to major releases
- Delete older runs as needed for space

## Archive Index

| Date | Run Name | Status | Notes |
|------|----------|--------|-------|
| | | | |
