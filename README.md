# SkillKit: Context Engineering

> **A self-contained quality-enforcement kit for Manual 3-CLI workflows.**

This kit enables consistent, high-quality development using a **Gemini → Codex → Claude** pipeline where a human operator manually coordinates three AI agents via copy-paste prompts.

## Quick Start

1. **Read**: [OPERATOR_GUIDE.md](./skillkit_context_engineering/OPERATOR_GUIDE.md) (the complete procedure)
2. **Create a Run**: Copy `ops/RUNS/_TEMPLATE_RUN` to `ops/RUNS/YYYY-MM-DD__task-name/`
3. **Execute**: Follow the 3-CLI workflow in the Operator Guide

## Directory Overview

| Directory | Purpose |
|-----------|---------|
| `spec/` | Immutable rules (core, constraints, interfaces, glossary, change policy) |
| `skills/` | Reusable skill procedures (authoring, coding, research, context) |
| `checklists/` | Atomic, testable quality items with tags and severity |
| `gates/` | Gate definitions (preflight, CI, semantic) + routing table |
| `eval/rubrics/` | Scoring rubrics for semantic quality checks |
| `ops/` | Operator workspace (prompts, runs, checkpoints) |
| `reports/` | Gate execution results and evidence |

## Core Principle

```
spec → checklist → gate → report → next cycle
```

Quality flows **one way**. Reports are evidence for the next cycle, not inputs to be modified.

## Integration

This kit is **non-invasive**. It does not restructure your existing project. To integrate:

1. Drop `skillkit_context_engineering/` at your repo root
2. Optionally add a link in your root README
3. Start using the workflow

---

**Version**: 1.0.0  
**License**: Internal Use
