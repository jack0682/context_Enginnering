# Operator Guide: Manual 3-CLI Workflow

> **This is the complete, executable procedure for running quality-enforced development using Gemini → Codex → Claude.**

---

## 1. Before You Start

### 1.1 System Requirements

| Requirement | How to Check | Install Command |
|-------------|--------------|-----------------|
| Gemini CLI | `gemini --version` | Follow Google AI instructions |
| Codex/OpenAI CLI | `openai --version` | `pip install openai` |
| Claude CLI | `claude --version` | Follow Anthropic instructions |
| Git | `git --version` | System package manager |
| Python 3.10+ | `python3 --version` | System package manager |

### 1.2 Terminal Setup

Open **exactly 3 terminal windows**. Label them clearly:

```
┌─────────────────┬─────────────────┬─────────────────┐
│   TERMINAL 1    │   TERMINAL 2    │   TERMINAL 3    │
│   ═══════════   │   ═══════════   │   ═══════════   │
│                 │                 │                 │
│   GEMINI        │   CODEX         │   CLAUDE        │
│   (Architect)   │   (Planner)     │   (Executor)    │
│                 │                 │                 │
│   cd /project   │   cd /project   │   cd /project   │
│   gemini        │   codex         │   claude        │
│                 │                 │                 │
└─────────────────┴─────────────────┴─────────────────┘
```

### 1.3 Verify Kit Installation

```bash
# Run from your project root
ls skillkit_context_engineering/
# Should show: OPERATOR_GUIDE.md  README.md  checklists/  eval/  gates/  ops/  reports/  skills/  spec/

# Verify core files exist
cat skillkit_context_engineering/spec/00_core.md | head -5
# Should show the core spec header
```

---

## 2. Creating a New Run

### 2.1 Step-by-Step Run Creation

```bash
# 1. Set the run name (use today's date + descriptive name)
export RUN_NAME="$(date +%Y-%m-%d)__add-user-auth"

# 2. Copy the template
cp -r skillkit_context_engineering/ops/RUNS/_TEMPLATE_RUN \
      skillkit_context_engineering/ops/RUNS/$RUN_NAME

# 3. Verify the copy
ls skillkit_context_engineering/ops/RUNS/$RUN_NAME/
# Should show: 00_casefile.md  01_inputs/  02_outputs/  03_notes.md

# 4. Open the casefile for editing
vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md
# Or use your preferred editor
```

### 2.2 Filling the Casefile — Concrete Example

Here's a real example of a filled casefile:

```markdown
# Casefile: Add JWT Authentication to REST API

## Metadata
| Field | Value |
|-------|-------|
| **Run ID** | `2026-01-02__add-user-auth` |
| **Created** | `2026-01-02T13:00:00+09:00` |
| **Status** | `OPEN` |
| **Work Type** | `feature` |

## 1. Objective
Add JWT-based authentication to the `/api/users` endpoints.

## 2. Context
Currently, all API endpoints are public. We need authentication 
before the v1.0 release. This is blocking ticket #123.

## 3. Constraints
- [x] Must not break existing API contracts (CON-010)
- [x] Must complete within 4 hours
- [x] Must use PyJWT library (already in requirements.txt)
- [x] No secrets in code (CON-050)

## 4. Scope
**IN scope:**
- Add `/auth/login` endpoint
- Add `/auth/refresh` endpoint  
- Protect `/api/users/*` endpoints

**OUT of scope:**
- OAuth integration (future)
- Password reset flow (ticket #124)

## 5. Input References
- `src/api/routes.py` — current routes
- `src/models/user.py` — User model
- JWT docs: https://pyjwt.readthedocs.io/

## 6. Success Criteria
- [ ] POST /auth/login returns JWT on valid credentials
- [ ] Protected endpoints return 401 without token
- [ ] Protected endpoints work with valid token
- [ ] All tests pass

## 7. Recommended Gates & Packs
| Gate | Required? |
|------|-----------|
| preflight | Yes |
| ci | Yes |
| semantic | Yes |

| Pack | Required? |
|------|-----------|
| pack-core | Yes |
```

---

## 3. Gemini Analysis Phase (Terminal 1)

### 3.1 The Prompt — Exact Copy-Paste

Open `skillkit_context_engineering/ops/PROMPTS/GEMINI_ANALYZE.md` and copy the **entire content**.

Before pasting, **replace `<RUN_NAME>` with your actual run name**:
- Find: `<RUN_NAME>`
- Replace with: `2026-01-02__add-user-auth` (your actual run name)

### 3.2 What Gemini Must Do

1. **Read these files** (Gemini does this automatically):
   - `skillkit_context_engineering/spec/00_core.md`
   - `skillkit_context_engineering/spec/01_constraints.md`
   - `skillkit_context_engineering/ops/RUNS/<RUN_NAME>/00_casefile.md`
   - Relevant source files mentioned in casefile

2. **Produce analysis** saved to:
   ```
   skillkit_context_engineering/ops/RUNS/<RUN_NAME>/02_outputs/gemini_analysis.md
   ```

### 3.3 Verify Gemini Output

After Gemini finishes, check the output:

```bash
# Check file exists
ls -la skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md

# Check it has required sections (should show all these headers)
grep "^## " skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md
# Expected output:
# ## 1. Request Summary
# ## 2. Architectural Impact
# ## 3. Constraints Check
# ## 4. High-Level Design
# ## 5. Risk Assessment
# ## 6. Open Questions
# ## 7. Recommended Checklist Pack
# ## 8. Handoff to Codex
```

### 3.4 Quality Check Before Proceeding

Before moving to Codex, verify:

| Check | Command | Expected |
|-------|---------|----------|
| File exists | `test -f .../gemini_analysis.md && echo OK` | `OK` |
| Has content | `wc -l .../gemini_analysis.md` | > 50 lines |
| Has constraints check | `grep -c "CON-" .../gemini_analysis.md` | > 0 |
| No FAIL constraints | `grep "FAIL" .../gemini_analysis.md` | Empty or explained |

**If any check fails, re-run Gemini with clarification.**

---

## 4. Codex Planning Phase (Terminal 2)

### 4.1 Pre-Codex Check

```bash
# Verify Gemini output is ready
cat skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md | head -20
```

### 4.2 The Prompt — Exact Copy-Paste

Open `skillkit_context_engineering/ops/PROMPTS/CODEX_PLAN.md` and copy the entire content.

Replace `<RUN_NAME>` with your actual run name.

### 4.3 What Codex Must Produce

1. **Read**:
   - `gemini_analysis.md` from step above
   - Existing code structure in repo

2. **Produce**:
   ```
   skillkit_context_engineering/ops/RUNS/<RUN_NAME>/02_outputs/codex_plan.md
   ```

### 4.4 Verify Codex Output

```bash
# Check required sections
grep "^## " skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md
# Expected:
# ## 1. Summary
# ## 2. File Changes
# ## 3. Directory Structure
# ## 4. Implementation Details
# ## 5. Dependency Order
# ## 6. Verification Plan
# ## 7. Rollback Plan
# ## 8. Handoff to Claude

# Check it has file changes
grep -E "\[NEW\]|\[MOD\]|\[DEL\]" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md
# Should list specific files
```

### 4.5 Quality Check Before Proceeding

| Check | Command | Expected |
|-------|---------|----------|
| Has file list | `grep -c "\.py\|\.ts\|\.md" .../codex_plan.md` | > 0 |
| Has skeletons | `grep -c "def \|class \|function " .../codex_plan.md` | > 0 |
| Has rollback | `grep -A5 "Rollback" .../codex_plan.md` | Has commands |

---

## 5. Claude Implementation Phase (Terminal 3)

### 5.1 Pre-Claude Check

```bash
# Verify Codex plan is ready
ls -la skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md
```

### 5.2 The Prompt — Exact Copy-Paste

Open `skillkit_context_engineering/ops/PROMPTS/CLAUDE_IMPLEMENT.md` and copy the entire content.

Replace `<RUN_NAME>` with your actual run name.

### 5.3 What Claude Must Do

1. **Read BEFORE modifying**:
   - Every file it plans to change
   - Document pre-change state in output

2. **Implement**:
   - Follow Codex plan exactly
   - Minimal diff — only change what's necessary
   - Check constraints for each change

3. **Collect Evidence**:
   - Run tests and save output
   - Run linters and save output
   - Save all logs to `01_inputs/`

4. **Produce**:
   ```
   skillkit_context_engineering/ops/RUNS/<RUN_NAME>/02_outputs/claude_impl.md
   ```
   Plus actual file changes in the repository.

### 5.4 Verify Claude Output

```bash
# Check implementation log exists and has content
wc -l skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md

# Check it has all required sections
grep "^## " skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md
# Expected:
# ## 1. Pre-Change Scan
# ## 2. Changes Applied
# ## 3. Evidence Collection
# ## 4. Constraint Compliance Check
# ## 5. Remaining Issues
# ## 6. Handoff Summary
# ## 7. Checklist Sign-off

# Check evidence was collected
ls skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/
# Should have: test.log, build.log, or similar evidence files
```

---

## 6. Gate Execution

### 6.1 Preflight Gate (L0) — MANDATORY

Run these checks manually and record results:

```bash
# === PREFLIGHT CHECKS ===

# PRE-001: Spec files exist
test -f skillkit_context_engineering/spec/00_core.md && echo "PRE-001: PASS" || echo "PRE-001: FAIL"
test -f skillkit_context_engineering/spec/01_constraints.md && echo "       PASS" || echo "       FAIL"

# PRE-002: Casefile exists
test -f skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md && echo "PRE-002: PASS" || echo "PRE-002: FAIL"

# PRE-003: Output directory exists
test -d skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs && echo "PRE-003: PASS" || echo "PRE-003: FAIL"

# PRE-004: No secrets in code (basic check)
grep -rn --include="*.py" --include="*.ts" -E "(api_key|password|secret|token)\s*=\s*['\"][^'\"]{8,}['\"]" . 2>/dev/null | grep -v test | head -5
# Should be empty or only test files
echo "PRE-004: $([ $? -eq 1 ] && echo PASS || echo REVIEW)"

# Save results
cat > skillkit_context_engineering/reports/latest/gate_preflight.json << EOF
{
  "gate": "preflight",
  "level": "L0",
  "run": "$RUN_NAME",
  "timestamp": "$(date -Iseconds)",
  "status": "PASS",
  "checks": [
    {"id": "PRE-001", "name": "Spec files exist", "status": "PASS"},
    {"id": "PRE-002", "name": "Casefile exists", "status": "PASS"},
    {"id": "PRE-003", "name": "Output dir exists", "status": "PASS"},
    {"id": "PRE-004", "name": "No secrets", "status": "PASS"}
  ],
  "blockers": [],
  "summary": "All preflight checks passed"
}
EOF
```

### 6.2 CI Gate (L1) — If You Have Tests

```bash
# === CI CHECKS ===

# Run your project's tests
pytest tests/ -v 2>&1 | tee skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/test.log
TEST_RESULT=$?

# Run syntax check
find . -name "*.py" -path "*/src/*" -exec python -m py_compile {} \; 2>&1 | tee skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/syntax.log
SYNTAX_RESULT=$?

# Record results
cat > skillkit_context_engineering/reports/latest/gate_ci.json << EOF
{
  "gate": "ci",
  "level": "L1",
  "run": "$RUN_NAME",
  "timestamp": "$(date -Iseconds)",
  "status": "$([ $TEST_RESULT -eq 0 ] && [ $SYNTAX_RESULT -eq 0 ] && echo PASS || echo FAIL)",
  "checks": [
    {"id": "CI-001", "name": "Syntax check", "status": "$([ $SYNTAX_RESULT -eq 0 ] && echo PASS || echo FAIL)"},
    {"id": "CI-004", "name": "Tests pass", "status": "$([ $TEST_RESULT -eq 0 ] && echo PASS || echo FAIL)"}
  ],
  "test_log": "ops/RUNS/$RUN_NAME/01_inputs/test.log",
  "blockers": []
}
EOF
```

### 6.3 Semantic Gate (L2) — Manual Review

For semantic checks, you must manually review and score:

```bash
# Open the rubrics and the output files side by side
# Score each criterion 0-3

# Example scoring for Evidence rubric:
# Read: skillkit_context_engineering/eval/rubrics/rubric-evidence.md
# Score: skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md

cat > skillkit_context_engineering/reports/latest/gate_semantic.json << EOF
{
  "gate": "semantic",
  "level": "L2",
  "run": "$RUN_NAME",
  "timestamp": "$(date -Iseconds)",
  "status": "PASS",
  "rubrics": {
    "evidence": {"existence": 3, "relevance": 2, "freshness": 3, "reproducibility": 2, "total": 10, "threshold": 8, "status": "PASS"},
    "logic": {"definitions": 3, "consistency": 3, "completeness": 2, "validity": 2, "total": 10, "threshold": 8, "status": "PASS"},
    "claims": {"traceability": 2, "precision": 2, "scope": 3, "falsifiability": 3, "total": 10, "threshold": 6, "status": "PASS"},
    "repro": {"commands": 3, "environment": 2, "data": 2, "randomness": 3, "total": 10, "threshold": 8, "status": "PASS"}
  }
}
EOF
```

---

## 7. Summary and Archive

### 7.1 Write Summary

```bash
cat > skillkit_context_engineering/reports/latest/summary.md << EOF
# Run Summary: $RUN_NAME

## Metadata
| Field | Value |
|-------|-------|
| **Run** | \`$RUN_NAME\` |
| **Completed** | \`$(date -Iseconds)\` |
| **Status** | \`SUCCESS\` |

## Gate Results
| Gate | Status | Blockers |
|------|--------|----------|
| Preflight (L0) | PASS | 0 |
| CI (L1) | PASS | 0 |
| Semantic (L2) | PASS | 0 |

## Changes Made
(Copy from claude_impl.md section 6.4)

## Next Steps
1. Deploy to staging
2. Manual QA verification
3. Merge to main branch

---
**Operator**: $(whoami)
EOF
```

### 7.2 Archive (After Merge/Completion)

```bash
# Create archive directory
ARCHIVE_DIR="skillkit_context_engineering/reports/archive/$(date +%Y-%m-%d)"
mkdir -p $ARCHIVE_DIR

# Copy reports
cp skillkit_context_engineering/reports/latest/* $ARCHIVE_DIR/

# Clear latest (optional)
# rm skillkit_context_engineering/reports/latest/*.json
# rm skillkit_context_engineering/reports/latest/summary.md
```

---

## 8. Failure Handling

### 8.1 BLOCKER Failure — STOP Immediately

```
┌─────────────────────────────────────────────────────┐
│  ⛔  BLOCKER FAILURE DETECTED                       │
│                                                     │
│  1. STOP all work immediately                       │
│  2. Document the failure:                           │
│     echo "BLOCKER: <check-id> failed" >> \          │
│       ops/CHECKPOINTS/$(date +%Y-%m-%d).md          │
│  3. Fix the blocker                                 │
│  4. Re-run from Step 1 (Gemini)                     │
│                                                     │
│  DO NOT PROCEED with blockers open.                 │
└─────────────────────────────────────────────────────┘
```

### 8.2 MAJOR Failure — Document and Plan

```bash
# Add to notes
echo "MAJOR: <check-id> failed - will fix in next cycle" >> \
  skillkit_context_engineering/ops/RUNS/$RUN_NAME/03_notes.md
```

### 8.3 Rollback Commands

```bash
# If implementation broke something:
git checkout HEAD -- src/  # Revert all changes in src/

# If specific files:
git checkout HEAD -- src/api/auth.py src/models/token.py

# Document the rollback
echo "$(date): Rolled back due to <reason>" >> \
  skillkit_context_engineering/ops/CHECKPOINTS/rollback-log.md
```

---

## 9. Quick Reference Card

```
╔═══════════════════════════════════════════════════════════════╗
║  MANUAL 3-CLI QUICK REFERENCE                                 ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  SETUP                                                        ║
║  ──────                                                       ║
║  export RUN_NAME="$(date +%Y-%m-%d)__task-name"               ║
║  cp -r ops/RUNS/_TEMPLATE_RUN ops/RUNS/$RUN_NAME              ║
║  vim ops/RUNS/$RUN_NAME/00_casefile.md                        ║
║                                                               ║
║  EXECUTION                                                    ║
║  ─────────                                                    ║
║  T1: Copy GEMINI_ANALYZE.md → run → gemini_analysis.md        ║
║  T2: Copy CODEX_PLAN.md → run → codex_plan.md                 ║
║  T3: Copy CLAUDE_IMPLEMENT.md → run → claude_impl.md + code   ║
║                                                               ║
║  GATES                                                        ║
║  ─────                                                        ║
║  L0 Preflight: spec exists, casefile ready, no secrets        ║
║  L1 CI: tests pass, syntax OK, builds                         ║
║  L2 Semantic: rubric scores ≥ threshold                       ║
║                                                               ║
║  SAVING EVIDENCE                                              ║
║  ──────────────                                               ║
║  pytest 2>&1 | tee ops/RUNS/$RUN_NAME/01_inputs/test.log      ║
║  make build 2>&1 | tee ops/RUNS/$RUN_NAME/01_inputs/build.log ║
║                                                               ║
║  FAILURE                                                      ║
║  ───────                                                      ║
║  BLOCKER → STOP → Fix → Restart from T1                       ║
║  MAJOR → Document → Continue → Fix next cycle                 ║
║  MINOR → Document → Continue                                  ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## 10. Troubleshooting

| Symptom | Likely Cause | Solution |
|---------|--------------|----------|
| Agent ignores casefile | Prompt doesn't include path | Check `<RUN_NAME>` replaced |
| Output in wrong location | Agent saved elsewhere | Re-run with explicit save command |
| Missing sections in output | Agent skipped format | Re-run, emphasize format requirement |
| Gate always fails | Check command incorrect | Review check logic |
| Secrets detected false positive | Pattern too broad | Add to exclude list |
| Tests fail after changes | Implementation bug | Use rollback, debug, retry |

---

**Version**: 1.0.0  
**Last Updated**: 2026-01-02
