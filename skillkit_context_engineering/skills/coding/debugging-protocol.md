# Skill: Debugging Protocol

## Purpose
Systematically isolate and fix bugs with evidence collection.

## When to Use
- Bug reports from users or tests
- Unexpected behavior during development
- Test failures after changes
- Production incidents

## Procedure

### Step 1: Reproduce the Bug
Before debugging, confirm reproduction:

```markdown
## Bug Reproduction

**Steps**:
1. [Step 1]
2. [Step 2]
3. [Step 3]

**Expected**: [What should happen]
**Actual**: [What actually happens]
**Reproducibility**: [Always / Sometimes / Once]
**Environment**: [OS, versions, config]
```

Record the reproduction command:
```bash
# Reproduction command
python -m pytest tests/test_x.py::test_failing -v 2>&1 | tee bug_repro.log
```

### Step 2: Isolate the Problem
Narrow down the cause:

```markdown
## Isolation

| Component | Tested | Works? |
|-----------|--------|--------|
| Database | `test_db_connection.py` | ✓ |
| API | `curl localhost:8000/health` | ✓ |
| Auth | `test_auth.py` | ✗ ← Problem here |
| Logic | — | Not tested yet |
```

Use binary search for large codebases:
1. Disable half the code
2. Check if bug persists
3. Repeat on the half that has the bug

### Step 3: Hypothesize and Test
Form hypotheses and test each:

| Hypothesis | Test | Result |
|------------|------|--------|
| Token expired | Check token timestamp | ✗ Not this |
| Wrong secret | Print secret comparison | ✓ Mismatch! |

### Step 4: Fix with Minimal Change
Apply the smallest fix possible:

```diff
# Before
- secret = os.getenv("SECRET")
+ secret = os.getenv("SECRET", "default_secret")
```

### Step 5: Verify Fix
1. Run the reproduction steps → should now pass
2. Run full test suite → no regressions
3. Document the fix

```markdown
## Resolution

**Root Cause**: SECRET env var not set in test environment
**Fix**: Added default value fallback
**Commit**: abc123
**Tests Added**: test_auth_with_default_secret
```

### Step 6: Add Regression Test
Create a test that would catch this bug if it returns:

```python
def test_auth_works_without_secret_env():
    """Regression test for bug #123."""
    with mock.patch.dict(os.environ, {"SECRET": ""}):
        result = auth.validate_token(token)
        assert result.valid
```

## Debug Evidence Collection

Always collect:
- [ ] Reproduction log (`bug_repro.log`)
- [ ] Stack trace (if applicable)
- [ ] Environment details
- [ ] Isolation test results
- [ ] Fix diff
- [ ] Regression test

## Checklist

| Step | Done? |
|------|-------|
| Bug reproduced reliably | |
| Problem isolated | |
| Root cause identified | |
| Minimal fix applied | |
| Fix verified | |
| Regression test added | |
| Evidence collected | |

## Common Mistakes

1. **Fix without reproducing**: Guessing at the problem
2. **Large fix**: Changing too much, introducing new bugs
3. **No regression test**: Same bug returns later
4. **Missing evidence**: Can't explain what happened
5. **Environment blind spots**: Works locally, fails in CI

## Related Constraints
- CON-020: Actions must be repeatable commands
