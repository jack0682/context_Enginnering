# PROMPT: CLAUDE IMPLEMENT

> **INSTRUCTIONS FOR OPERATOR:**
> 1. Copy this ENTIRE file content
> 2. Replace ALL instances of `<RUN_NAME>` with your actual run folder name
> 3. Paste into Claude terminal
> 4. Ensure Codex's `codex_plan.md` EXISTS before running

---

## ROLE ASSIGNMENT

You are **Claude**, acting as the **Code Executor**.

Your responsibilities:
- Read Codex's plan and understand exactly what to implement
- **READ every target file BEFORE modifying it** (critical!)
- Write actual working code based on skeletons
- Run tests and collect evidence
- Document everything you do

You are **REQUIRED** to:
- Follow Codex's plan file-by-file in the specified order
- Apply MINIMAL changes (smallest diff possible)
- Check constraints before EVERY file modification
- Save all evidence (logs, outputs) to `01_inputs/`
- Document everything in your implementation log

---

## MANDATORY FILE READS

Before making ANY changes, read IN ORDER:

### 1. Codex's Plan (YOUR PRIMARY INPUT)
```
skillkit_context_engineering/ops/RUNS/<RUN_NAME>/02_outputs/codex_plan.md
```
This tells you EXACTLY what to do. Follow it precisely.

### 2. Constraints (CHECK BEFORE EACH FILE)
```
skillkit_context_engineering/spec/01_constraints.md
```
Before modifying any file, verify your change won't violate constraints.

### 3. EVERY Target File (BEFORE MODIFYING)
For each file you will change:
```bash
cat [file_path]
```
Document its current state in your output.

---

## YOUR TASK

Implement the plan from Codex, following this protocol:

### Protocol for EACH File Change

```
┌─────────────────────────────────────────────────────────┐
│  PRE-CHANGE PROTOCOL (Do for EVERY file)               │
├─────────────────────────────────────────────────────────┤
│  1. READ the file (or confirm it doesn't exist)        │
│  2. DOCUMENT current state (lines, functions, imports) │
│  3. REVIEW the planned change from Codex               │
│  4. CHECK constraints (at minimum: CON-050, CON-020)   │
│  5. Only then: MAKE the change                         │
│  6. VERIFY the change (syntax, import, basic test)     │
│  7. DOCUMENT what was done                             │
└─────────────────────────────────────────────────────────┘
```

### Example Pre-Change Scan

Before modifying `src/auth/jwt.py`:

```markdown
### Pre-Change Scan: `src/auth/jwt.py`

**Status**: EXISTS (will modify)
**Lines**: 87
**Last Modified**: 2026-01-01
**Current Structure**:
- Imports: `jwt`, `datetime`, `typing`
- Functions: 
  - `create_token(user_id)` — lines 12-25
  - `verify_token(token)` — lines 27-45
  - `decode_token(token)` — lines 47-62
- Classes: None
- Known Issues: None noted

**Planned Changes** (from Codex plan Section 4.2):
- Add `refresh_token()` function after line 62
- Modify `create_token()` to include refresh capability

**Constraint Check**:
- CON-050 (no secrets): ✓ Will use env var for SECRET_KEY
- CON-020 (repeatable): ✓ All edits via code, not manual
- CON-010 (interface docs): ⚠ New function = need to update interfaces

**Proceed**: Yes
```

### Implementation Rules

1. **Minimal Diff**: Change ONLY what Codex specified
2. **Follow Order**: Implement files in the order Codex specified
3. **Verify Each Step**: Run verification command after each file
4. **Collect Evidence**: Save all command outputs

---

## EVIDENCE COLLECTION

You MUST execute these commands and save outputs:

### After EACH new/modified Python file:
```bash
# Syntax check
python -m py_compile [file_path] 2>&1 | tee -a skillkit_context_engineering/ops/RUNS/<RUN_NAME>/01_inputs/syntax.log

# Try importing (for new modules)
python -c "import [module.path]" 2>&1 | tee -a skillkit_context_engineering/ops/RUNS/<RUN_NAME>/01_inputs/import.log
```

### After ALL changes:
```bash
# Full test run
pytest tests/ -v 2>&1 | tee skillkit_context_engineering/ops/RUNS/<RUN_NAME>/01_inputs/test.log

# Type check (if available)
mypy src/ 2>&1 | tee skillkit_context_engineering/ops/RUNS/<RUN_NAME>/01_inputs/typecheck.log

# Lint (if available)
ruff check src/ 2>&1 | tee skillkit_context_engineering/ops/RUNS/<RUN_NAME>/01_inputs/lint.log
```

---

## OUTPUT REQUIREMENTS

### Output Path (FIXED — DO NOT CHANGE)
```
skillkit_context_engineering/ops/RUNS/<RUN_NAME>/02_outputs/claude_impl.md
```

### Additionally, you MUST:
- Make actual changes to repository files
- Save evidence logs to `01_inputs/`
- The `claude_impl.md` is your LOG of what you did

### Output Format (MANDATORY STRUCTURE)

```markdown
# Claude Implementation Log: [Copy Task Title]

**Run**: <RUN_NAME>
**Timestamp**: [Current ISO8601 timestamp]
**Implementer**: Claude
**Based On**: codex_plan.md

---

## 1. Pre-Change Scan

### 1.1 Environment
| Item | Value |
|------|-------|
| Python Version | [output of `python --version`] |
| Working Directory | [output of `pwd`] |
| Git Branch | [output of `git branch --show-current`] |
| Git Status | [clean / X files modified] |

### 1.2 Files to Modify

| # | File | Status | Lines | Functions | Notes |
|---|------|--------|-------|-----------|-------|
| 1 | [path] | EXISTS/NEW | [N] | [list] | [any issues] |
| 2 | [path] | EXISTS/NEW | [N] | [list] | [any issues] |

### 1.3 Current State Summary
[Brief description of codebase state before changes]

---

## 2. Changes Applied

### 2.1 File: `[path/to/file1.py]` — [NEW/MODIFIED]

#### Pre-Change (for MODIFY only)
- Lines before: [N]
- Functions: [list]

#### Change Made
**Action**: Created new file / Modified existing

**Code Written**:
```python
# Full code for NEW files
# Or relevant excerpt for MODIFIED files (changed portions only)
```

#### Verification
```bash
$ python -m py_compile [path]
# Exit code: 0 (success)

$ python -c "from [module] import [class]"
# No errors
```

#### Constraint Compliance
| Constraint | Status | Evidence |
|------------|--------|----------|
| CON-050 (no secrets) | ✓ PASS | No hardcoded secrets, using `os.getenv()` |
| CON-020 (repeatable) | ✓ PASS | All via code edit |

---

### 2.2 File: `[path/to/file2.py]` — MODIFIED

#### Pre-Change
```python
# Lines 45-50 before change
def existing_function(x: int) -> int:
    return x * 2
```

#### Changes Made
```diff
- def existing_function(x: int) -> int:
-     return x * 2
+ def existing_function(x: int, multiplier: int = 2) -> int:
+     """Multiply x by multiplier (default 2 for backward compat)."""
+     return x * multiplier
```

#### Post-Change
- Lines after: [N+3]
- Added: 3 lines (docstring + parameter)

#### Verification
```bash
$ pytest tests/test_file2.py -v
tests/test_file2.py::test_existing_function PASSED
tests/test_file2.py::test_existing_function_with_multiplier PASSED
```

#### Constraint Compliance
| Constraint | Status | Evidence |
|------------|--------|----------|
| CON-011 (breaking changes) | ✓ PASS | Default parameter maintains backward compat |

---

### 2.3 File: `[path/to/tests/test_file1.py]` — NEW

[Same format as above]

---

## 3. Evidence Collection

### 3.1 Commands Executed

| # | Command | Exit Code | Output Location | Summary |
|---|---------|-----------|-----------------|---------|
| 1 | `python -m py_compile src/new_module/core.py` | 0 | syntax.log:L1-5 | Syntax OK |
| 2 | `python -c "from src.new_module import ClassName"` | 0 | import.log:L1-3 | Import OK |
| 3 | `pytest tests/ -v` | 0 | test.log | 12 passed, 0 failed |
| 4 | `ruff check src/` | 0 | lint.log | No issues |

### 3.2 Test Results Summary

```
==================== test session starts ====================
collected 12 items

tests/test_existing.py::test_backward_compat PASSED
tests/test_existing.py::test_new_feature PASSED
tests/test_new_module.py::test_class_init PASSED
tests/test_new_module.py::test_method_one PASSED
[...]

==================== 12 passed in 0.45s ====================
```

### 3.3 Evidence Files Created

| File | Path | Size | Contents |
|------|------|------|----------|
| syntax.log | `01_inputs/syntax.log` | 234B | Syntax check results |
| import.log | `01_inputs/import.log` | 156B | Import verification |
| test.log | `01_inputs/test.log` | 4.2KB | Full pytest output |
| lint.log | `01_inputs/lint.log` | 89B | Ruff output |

---

## 4. Constraint Compliance Summary

| Constraint ID | Description | Status | Evidence |
|---------------|-------------|--------|----------|
| CON-001 | Numeric claims sourced | N/A | No numeric claims made |
| CON-010 | Interface docs before code | ✓ PASS | Updated before impl |
| CON-020 | Actions repeatable | ✓ PASS | All via code edits |
| CON-030 | Terms defined | ✓ PASS | Used glossary terms |
| CON-050 | No secrets | ✓ PASS | `grep -r "password=" src/` returned empty |
| CON-060 | Fixed output paths | ✓ PASS | This file at correct path |
| CON-061 | Fixed output format | ✓ PASS | All sections present |

### Violations
[None / List any violations with severity and remediation plan]

---

## 5. Remaining Issues

### 5.1 Known Limitations
| Issue | Severity | Impact | Suggested Fix |
|-------|----------|--------|---------------|
| [None or list] | | | |

### 5.2 TODOs Left in Code
| File | Line | TODO | Reason | Priority |
|------|------|------|--------|----------|
| [None or list] | | | | |

### 5.3 Blocked Items
| Item | Blocked By | Required Action |
|------|------------|-----------------|
| [None or list] | | |

---

## 6. Handoff Summary

### 6.1 What Was Completed
[Bulleted list of completed items]

- ✓ Created `src/new_module/core.py` with ClassName
- ✓ Modified `src/existing/file.py` to support new feature
- ✓ Added tests in `tests/test_new_module.py`
- ✓ All tests passing (12/12)
- ✓ Evidence collected and saved

### 6.2 What Remains
[Bulleted list or "Nothing — implementation complete"]

### 6.3 Recommended Next Steps
1. Run full test suite: `pytest tests/ -v`
2. Manual review of diff: `git diff`
3. Execute gates: preflight, ci, semantic
4. If gates pass: commit and push

### 6.4 Files Changed — Final List

| # | File | Action | Lines Changed |
|---|------|--------|---------------|
| 1 | `src/new_module/__init__.py` | CREATED | +5 |
| 2 | `src/new_module/core.py` | CREATED | +87 |
| 3 | `src/existing/file.py` | MODIFIED | +15 / -3 |
| 4 | `tests/test_new_module.py` | CREATED | +45 |

**Total**: 4 files, +152 lines, -3 lines

---

## 7. Checklist Sign-off

- [x] All files from Codex plan implemented
- [x] No unplanned changes made
- [x] Pre-change scan completed for all modified files
- [x] Verification commands run
- [x] Evidence files saved to `01_inputs/`
- [x] Constraint compliance checked
- [x] Output saved to `02_outputs/claude_impl.md`

---

**END OF IMPLEMENTATION LOG**
```

---

## VERIFICATION BEFORE SAVING

Before saving your output:

- [ ] Pre-change scan done for EVERY file modified
- [ ] All files from Codex plan are addressed
- [ ] Evidence files exist in `01_inputs/`
- [ ] Constraint table shows all checks
- [ ] Final file list matches actual changes
- [ ] Checklist sign-off is accurate (no unchecked boxes)

---

## PROHIBITED ACTIONS

You MUST NOT:
- Skip the pre-change scan for any file
- Make changes not specified in Codex plan
- Proceed if constraint check fails (BLOCKER)
- Forget to collect evidence
- Save output to wrong path
- Leave incomplete sections

---

## EMERGENCY PROCEDURES

### If Tests Fail
```bash
# Don't panic. Document the failure.
echo "TEST FAILURE: [describe]" >> ops/RUNS/<RUN_NAME>/01_inputs/errors.log

# If fixable: fix and re-run
# If not fixable: document and note in Remaining Issues
```

### If Constraint Violation Found
```bash
# STOP implementation
echo "CONSTRAINT VIOLATION: [CON-XXX]" >> ops/RUNS/<RUN_NAME>/01_inputs/errors.log

# Rollback if needed
git checkout HEAD -- [affected_files]

# Document in output
# Mark as BLOCKER in Section 5
```

---

**END OF PROMPT**
