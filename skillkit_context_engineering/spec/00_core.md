# 00. Core Specification

> **Purpose**: This document defines the fundamental identity, goals, and boundaries of the SkillKit and any project using it.

---

## 1. Kit Identity

### 1.1 Name
**SkillKit: Context Engineering**

### 1.2 One-Sentence Definition
A self-contained quality-enforcement kit for manual 3-CLI AI agent workflows that ensures consistent, high-quality development through spec-checklist-gate-report cycles.

### 1.3 Version
**1.0.0**

---

## 2. Purpose

This kit exists to:

1. **Enforce Quality**: Prevent low-quality outputs by requiring evidence-based development
2. **Enable Reproducibility**: Ensure any operator can repeat a workflow with the same results
3. **Standardize Handoffs**: Fix output paths and formats so agents can consume each other's outputs reliably
4. **Document Everything**: Create an audit trail for every change

---

## 3. Done Definition

A task is **DONE** when:

| Criterion | Evidence Required |
|-----------|-------------------|
| All planned changes implemented | `claude_impl.md` lists all files + actions |
| No BLOCKER failures | `gate_preflight.json` shows no blockers |
| CI passes (if applicable) | `gate_ci.json` shows PASS |
| Semantic checks pass | `gate_semantic.json` shows PASS |
| Summary written | `reports/latest/summary.md` exists |
| Evidence archived | Previous reports moved to `reports/archive/` |

---

## 4. Non-Goals

This kit is **NOT** for:

| Non-Goal | Reason |
|----------|--------|
| Automated orchestration | Designed for manual 3-CLI operation |
| Runtime execution | Kit is for development workflow, not runtime |
| Replacing existing CI/CD | Kit supplements, not replaces, existing pipelines |
| Language-specific linting | Use your project's linters; kit focuses on semantic quality |
| Credential management | No secrets handled; use external secret managers |

---

## 5. Scope

### 5.1 In Scope
- Spec definition and enforcement
- Checklist creation and verification
- Gate definition and result recording
- Rubric-based semantic evaluation
- Operator workflow documentation
- Evidence collection and archival

### 5.2 Out of Scope
- Actual code implementation (that's your project)
- Deployment pipelines
- Monitoring and alerting
- User authentication

---

## 6. Integration Model

The kit integrates with projects via **minimal intrusion**:

```
your_project/
├── src/                    # Your code (untouched)
├── tests/                  # Your tests (untouched)
├── ...                     # Your other files (untouched)
└── skillkit_context_engineering/   # The kit (self-contained)
```

The kit does NOT:
- Move your existing files
- Require changes to your build system
- Impose a specific language or framework

---

## 7. Quality Flow

The fundamental flow is **one-way**:

```
spec → checklist → gate → report
         ↑                    │
         └────────────────────┘
              (next cycle)
```

- **Spec** defines rules (immutable within a version)
- **Checklist** operationalizes rules into testable items
- **Gate** groups checklist items for execution
- **Report** records evidence of gate execution
- **Next cycle** uses reports as input context (not as editable artifacts)

---

## 8. Core Invariants

These must ALWAYS hold:

| ID | Invariant | Violation = |
|----|-----------|-------------|
| INV-001 | Outputs go to fixed paths | BLOCKER |
| INV-002 | Outputs follow fixed formats | BLOCKER |
| INV-003 | Specs are read-only during execution | BLOCKER |
| INV-004 | Evidence is collected for every claim | MAJOR |
| INV-005 | BLOCKER failures stop the pipeline | BLOCKER |

---

## 9. Versioning

This kit follows [Semantic Versioning](https://semver.org/):

- **MAJOR**: Breaking changes to spec, checklist structure, or output formats
- **MINOR**: New features, new checklist items, new gates (backward compatible)
- **PATCH**: Bug fixes, documentation improvements

Current version: **1.0.0**

---

## 10. Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-01-02 | Initial release |

---

**END OF DOCUMENT**
