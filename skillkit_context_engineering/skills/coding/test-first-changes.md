# Skill: Test-First Changes

## Purpose
Ensure changes are verified by tests before they are considered complete.

## When to Use
- Adding new features
- Fixing bugs
- Refactoring code
- Modifying behavior

## Procedure

### Step 1: Define Expected Behavior
Before writing code, document:
- What should happen (success cases)
- What should not happen (failure cases)
- Edge cases to consider

```markdown
## Expected Behavior: User Authentication

### Success Cases
- Valid credentials → returns session token
- Token format: JWT with 1hr expiry

### Failure Cases
- Invalid password → returns 401
- Missing user → returns 401 (same as invalid password for security)
- Expired token → returns 403

### Edge Cases
- Empty password → returns 400
- Very long password (>1000 chars) → returns 400
```

### Step 2: Write Failing Tests First
Create tests that fail (because the feature doesn't exist yet):

```python
# test_auth.py
def test_valid_login_returns_token():
    response = auth.login("user@example.com", "correct_password")
    assert response.status == 200
    assert "token" in response.body
    
def test_invalid_password_returns_401():
    response = auth.login("user@example.com", "wrong_password")
    assert response.status == 401
```

### Step 3: Implement Code
Write the minimum code to make tests pass.

### Step 4: Verify All Tests Pass
```bash
# Run tests
pytest tests/test_auth.py -v

# Expected output
test_valid_login_returns_token PASSED
test_invalid_password_returns_401 PASSED
```

### Step 5: Check Coverage
```bash
pytest --cov=src/auth --cov-report=term-missing
```

Ensure:
- [ ] All success paths covered
- [ ] All failure paths covered
- [ ] Edge cases covered

## Test Categories

| Category | Purpose | Required? |
|----------|---------|-----------|
| Unit | Test individual functions | Yes |
| Integration | Test component interactions | For changes crossing modules |
| End-to-End | Test full user flows | For user-facing changes |
| Regression | Prevent old bugs | For bug fixes |

## Test Naming Convention

```
test_<unit>_<scenario>_<expected>

Examples:
test_login_valid_credentials_returns_token
test_login_missing_password_returns_400
test_user_delete_nonexistent_returns_404
```

## Checklist

| Item | Status |
|------|--------|
| Expected behavior documented | |
| Failing tests written first | |
| Code implemented | |
| All tests pass | |
| Coverage adequate | |
| No test skips without justification | |

## Common Mistakes

1. **Tests after code**: Writing tests to match existing code, not requirements
2. **Happy path only**: Testing success but not failure
3. **Test pollution**: Tests depend on each other's state
4. **Mocking too much**: Tests pass but real integration fails
5. **Skipping edge cases**: Common bugs hide in edge cases

## Related Constraints
- CON-020: Actions must be repeatable
