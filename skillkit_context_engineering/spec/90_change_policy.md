# 90. Change Policy

> **Purpose**: This document defines how changes to specs, checklists, gates, and other kit components are managed.

---

## 1. Change Categories

### 1.1 Spec Changes (`spec/*.md`)
- **Authority**: Human Architect only
- **Process**: Propose → Review → Approve → Merge
- **Version Impact**: Usually MAJOR or MINOR

### 1.2 Checklist Changes (`checklists/`)
- **Authority**: Human Architect or designated reviewer
- **Process**: Propose → Review → Test → Merge
- **Version Impact**: MINOR for additions, MAJOR for removals

### 1.3 Gate Changes (`gates/`)
- **Authority**: Human Architect or designated reviewer
- **Process**: Propose → Review → Test → Merge
- **Version Impact**: MINOR for additions, MAJOR for breaking changes

### 1.4 Prompt Changes (`ops/PROMPTS/`)
- **Authority**: Human Architect
- **Process**: Propose → Test with agents → Review output quality → Merge
- **Version Impact**: MINOR

### 1.5 Operational Changes (`ops/RUNS/`, `reports/`)
- **Authority**: Any operator
- **Process**: No formal review (these are working artifacts)
- **Version Impact**: None (not versioned)

---

## 2. Versioning Rules

### 2.1 Semantic Versioning
This kit follows [Semantic Versioning 2.0.0](https://semver.org/):

```
MAJOR.MINOR.PATCH

MAJOR: Breaking changes
MINOR: New features, backward compatible
PATCH: Bug fixes, documentation
```

### 2.2 What Counts as Breaking?
| Change | Breaking? |
|--------|-----------|
| Remove constraint | Yes |
| Change constraint severity (MAJOR→BLOCKER) | Yes |
| Add new constraint | No |
| Change output format structure | Yes |
| Add optional output field | No |
| Remove checklist item | Yes |
| Add checklist item | No |
| Change gate composition | Depends |

---

## 3. Change Proposal Process

### 3.1 Template

Create a proposal in `ops/CHECKPOINTS/proposals/YYYY-MM-DD__proposal-name.md`:

```markdown
# Change Proposal: [Title]

**Date**: YYYY-MM-DD
**Author**: [Name]
**Status**: PROPOSED / APPROVED / REJECTED / IMPLEMENTED

## 1. Summary
(One paragraph describing the change)

## 2. Motivation
(Why is this change needed?)

## 3. Proposed Change
(Specific changes to make)

## 4. Impact Analysis
- Affected components: [list]
- Breaking changes: yes/no
- Version bump: MAJOR/MINOR/PATCH

## 5. Rollback Plan
(How to undo if needed)

## 6. Approval
- [ ] Reviewed by: [name]
- [ ] Approved by: [name]
- [ ] Implemented by: [name]
```

### 3.2 Review Criteria
- Does the change align with `00_core.md`?
- Does it violate any constraint in `01_constraints.md`?
- Is the impact analysis complete?
- Is the rollback plan feasible?

---

## 4. Rollback Policy

### 4.1 When to Rollback
- Gate failure after change
- Unexpected agent behavior
- Quality regression in outputs

### 4.2 How to Rollback
```bash
# 1. Identify the bad commit/change
git log --oneline -10

# 2. Revert the change
git revert <commit-hash>

# 3. Document the rollback
echo "Rolled back commit <hash> due to: <reason>" >> ops/CHECKPOINTS/rollback-log.md

# 4. Re-run affected gates
```

### 4.3 Rollback Evidence
Every rollback must be documented in `ops/CHECKPOINTS/rollback-log.md`:

```markdown
## Rollback: YYYY-MM-DD HH:MM

**Reverted**: [commit or change description]
**Reason**: [why]
**Evidence**: [gate failure, log excerpt, etc.]
**Operator**: [who performed rollback]
```

---

## 5. Compatibility Rules

### 5.1 Backward Compatibility
- MINOR versions must be backward compatible with the previous MINOR
- Projects using kit version 1.x should work with any 1.y (y > x)

### 5.2 Forward Compatibility
- Not guaranteed
- Projects should pin to a specific kit version for stability

### 5.3 Migration Guides
For MAJOR version bumps, a migration guide must be provided:

```
skillkit_context_engineering/
└── migrations/
    └── v1-to-v2.md
```

---

## 6. Deprecation Process

### 6.1 Timeline
1. **Announce**: Mark as deprecated in documentation
2. **Warn**: Add warnings to affected components
3. **Remove**: Remove after at least one MINOR version cycle

### 6.2 Deprecation Notice Format
```markdown
> ⚠️ **DEPRECATED** as of v1.5.0
> This [item] will be removed in v2.0.0.
> Use [alternative] instead.
```

---

## 7. Emergency Changes

### 7.1 Definition
Changes required to fix:
- Security vulnerabilities
- BLOCKER-severity bugs
- Data loss issues

### 7.2 Process
1. **Identify**: Confirm emergency status
2. **Fix**: Apply minimal fix
3. **Test**: Run critical gates only
4. **Deploy**: Merge immediately
5. **Document**: Post-mortem within 24 hours

### 7.3 Post-Emergency
- Full gate run within 48 hours
- Update constraints/checklists if gap identified
- Add test case to prevent recurrence

---

## 8. Audit Trail

All changes to versioned components must be tracked:

| Date | Component | Change | Author | Version |
|------|-----------|--------|--------|---------|
| 2026-01-02 | Initial | Created kit | Architect | 1.0.0 |

---

**END OF DOCUMENT**
