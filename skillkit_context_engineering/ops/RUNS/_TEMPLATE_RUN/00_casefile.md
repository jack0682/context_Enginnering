# Casefile: [Replace with Task Title]

> **Instructions**:
> 1. Copy this template to a new run folder: `ops/RUNS/YYYY-MM-DD__task-name/`
> 2. Fill in ALL sections below (delete placeholder text)
> 3. This becomes the input for Gemini analysis

---

## Metadata

| Field | Value |
|-------|-------|
| **Run ID** | `[YYYY-MM-DD__task-name]` |
| **Created** | `[YYYY-MM-DDTHH:MM:SS+TZ]` |
| **Status** | `OPEN` |
| **Work Type** | `[feature / bugfix / refactor / docs / experiment / paper]` |
| **Estimated Duration** | `[X hours]` |
| **Priority** | `[P0-Critical / P1-High / P2-Medium / P3-Low]` |

---

## 1. Objective

### 1.1 Primary Goal
> Write ONE clear sentence describing what must be accomplished.

**Example**: "Add JWT-based authentication to protect `/api/users/*` endpoints."

**Your Objective**: [Write here]

### 1.2 Success Criteria
> How will we know this is done? Be specific and testable.

| # | Criterion | Verification Method |
|---|-----------|---------------------|
| 1 | [E.g., "Login endpoint returns JWT"] | [E.g., "curl test returns 200 + token"] |
| 2 | [E.g., "Protected routes return 401 without token"] | [E.g., "curl without auth header"] |
| 3 | [E.g., "All tests pass"] | [E.g., "pytest exits 0"] |

### 1.3 Acceptance Test
> What exact command or sequence proves completion?

```bash
# Example:
curl -X POST http://localhost:8000/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "test", "password": "test123"}' \
  | jq '.token'
# Expected: Returns JWT string

# Your acceptance test:
[Write command here]
```

---

## 2. Context

### 2.1 Background
> Why is this work needed? What problem does it solve?

[Write 2-5 sentences]

### 2.2 Related Work
> Links to related tickets, PRs, documents, or previous runs.

| Reference | Link/Path | Relevance |
|-----------|-----------|-----------|
| [Ticket #] | [URL or path] | [How it relates] |
| [Previous run] | [ops/RUNS/...] | [What was done] |
| [Design doc] | [path] | [Relevant sections] |

### 2.3 Current State
> What exists now? What doesn't work?

```
Current behavior:
- [Describe what happens now]
- [What's missing or broken]

Desired behavior:
- [What should happen after this task]
```

---

## 3. Scope

### 3.1 In Scope
> List specific items that WILL be addressed.

- [ ] [Item 1 - specific and actionable]
- [ ] [Item 2 - specific and actionable]
- [ ] [Item 3 - specific and actionable]

### 3.2 Out of Scope
> List items explicitly NOT addressed (to prevent scope creep).

| Item | Reason | Future Ticket? |
|------|--------|----------------|
| [Item A] | [Why not now] | [Yes/No - #XXX] |
| [Item B] | [Why not now] | [Yes/No] |

### 3.3 Dependencies
> What must exist before this work can proceed?

| Dependency | Status | Blocker? |
|------------|--------|----------|
| [E.g., "User model exists"] | ✓ Ready | No |
| [E.g., "Database migrations"] | ⏳ Pending | Yes |

---

## 4. Constraints

### 4.1 Technical Constraints
> Hard limits on the implementation.

- [ ] Language/Framework: [E.g., "Must use Python 3.10+, FastAPI"]
- [ ] Performance: [E.g., "Response time < 100ms"]
- [ ] Compatibility: [E.g., "Must work with existing v1 API clients"]
- [ ] Security: [E.g., "No secrets in code, use environment variables"]

### 4.2 Process Constraints
> Rules from spec/01_constraints.md that especially apply.

| Constraint ID | Description | How It Applies |
|---------------|-------------|----------------|
| CON-010 | Interface docs before code | New API endpoints need interface doc |
| CON-020 | Actions must be commands | All changes via git-trackable edits |
| CON-050 | No secrets | JWT secret via env var |

### 4.3 Time/Resource Constraints

| Constraint | Value |
|------------|-------|
| Deadline | [Date or "None"] |
| Max effort | [X hours] |
| Must complete by | [Date/Time or "Flexible"] |

---

## 5. Input References

### 5.1 Files to Read
> Which files should agents examine?

| File | Purpose | Key Sections |
|------|---------|--------------|
| `src/api/routes.py` | Current routes | Lines 20-50 |
| `src/models/user.py` | User model | Entire file |
| `requirements.txt` | Dependencies | Check for pyjwt |

### 5.2 External Documentation
> External docs to reference.

| Documentation | URL | What to Extract |
|---------------|-----|-----------------|
| [PyJWT docs] | [url] | Token creation/verification |
| [FastAPI security] | [url] | OAuth2 password flow |

### 5.3 Examples/Templates
> Similar implementations to reference.

```
Reference implementation: [path or URL]
Relevant pattern: [what to copy/adapt]
```

---

## 6. Impact Analysis

> **Required if scope affects 3+ modules** (CON-X01)

### 6.1 Affected Components

| Component | Path | Impact Type | Risk |
|-----------|------|-------------|------|
| API Routes | `src/api/routes.py` | MODIFY | Low |
| Auth Module | `src/auth/` | NEW | Med |
| Tests | `tests/test_auth.py` | NEW | Low |
| User Model | `src/models/user.py` | MODIFY | Med |

### 6.2 Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Break existing API | Low | High | Keep backward compat |
| Token security issue | Med | High | Use established library |
| Test coverage gap | Med | Med | Add comprehensive tests |

### 6.3 Rollback Plan
> If this fails, how do we undo?

```bash
# Rollback commands:
git checkout HEAD -- src/api/ src/auth/ src/models/
git clean -fd src/auth/  # Remove new files

# Verify rollback:
pytest tests/ -v
```

---

## 7. Recommended Configuration

### 7.1 Gates to Run

| Gate | Required | Reason |
|------|----------|--------|
| Preflight (L0) | ✓ Yes | Always required |
| CI (L1) | ✓ Yes | New code needs tests |
| Semantic (L2) | ○ Optional | If complex logic |

### 7.2 Checklist Packs

| Pack | Required | Reason |
|------|----------|--------|
| pack-core | ✓ Yes | Always required |
| pack-docs | ○ If API docs | New endpoints |
| pack-paper | ✗ No | Not academic |
| pack-ros2 | ✗ No | Not robotics |

### 7.3 Skills to Apply

| Skill | Path | When to Use |
|-------|------|-------------|
| contract-safety | `skills/coding/contract-safety.md` | Adding new API |
| test-first | `skills/coding/test-first-changes.md` | Writing tests |

---

## 8. Handoff Tracking

> Track completion of each phase.

| Phase | Status | Output Path | Completed |
|-------|--------|-------------|-----------|
| Casefile | ✓ DONE | This file | [timestamp] |
| Gemini Analysis | ⏳ Pending | `02_outputs/gemini_analysis.md` | |
| Codex Plan | ⏳ Pending | `02_outputs/codex_plan.md` | |
| Claude Implementation | ⏳ Pending | `02_outputs/claude_impl.md` | |
| Gate: Preflight | ⏳ Pending | `reports/latest/gate_preflight.json` | |
| Gate: CI | ⏳ Pending | `reports/latest/gate_ci.json` | |
| Gate: Semantic | ⏳ Pending | `reports/latest/gate_semantic.json` | |
| Summary | ⏳ Pending | `reports/latest/summary.md` | |

---

## 9. Notes

> Free-form notes, questions, decisions.

### Questions to Resolve
- [ ] [Question 1]
- [ ] [Question 2]

### Decisions Made
| Decision | Rationale | Date |
|----------|-----------|------|
| | | |

### Open Issues
- 

---

## Checklist Before Starting

Before passing this to Gemini, verify:

- [ ] Objective is clear and actionable
- [ ] Success criteria are testable
- [ ] Scope is explicitly defined (in AND out)
- [ ] Constraints from spec are referenced
- [ ] Input files are listed
- [ ] Impact analysis complete (if 3+ modules)

---

**END OF CASEFILE**
