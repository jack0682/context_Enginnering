# Skill: Contract Safety

## Purpose
Ensure code changes respect interface contracts and maintain API stability.

## When to Use
- Modifying public functions or classes
- Changing API endpoints
- Updating data schemas
- Refactoring shared code

## Procedure

### Step 1: Identify Contracts
Before changing code, identify all contracts:
- [ ] Function signatures (parameters, return types)
- [ ] API endpoints (routes, request/response shapes)
- [ ] Data schemas (field names, types, required/optional)
- [ ] Event formats (message shapes, topics)
- [ ] File formats (structure, encoding)

### Step 2: Assess Impact
For each contract:

| Contract | Current | Proposed Change | Breaking? |
|----------|---------|-----------------|-----------|
| `get_user(id: int)` | Returns User | Add optional param | No |
| `/api/users` | Returns `{users: [...]}` | Add `total` field | No |
| `/api/users` | `id` is string | `id` becomes int | **Yes** |

### Step 3: Apply Change Rules

**Safe Changes** (backward compatible):
- Adding optional parameters with defaults
- Adding new fields to responses
- Adding new endpoints
- Relaxing validation (accepting more inputs)

**Breaking Changes** (require version bump):
- Removing parameters or fields
- Changing types
- Renaming anything public
- Tightening validation (rejecting previously valid inputs)

### Step 4: Update Interface Docs
If changing any contract:
1. Update `spec/02_interfaces.md` FIRST
2. Increment version if breaking
3. Add migration notes if needed
4. THEN implement the change

### Step 5: Add Compatibility Tests
For every contract change, add tests:

```python
# test_api_compatibility.py
def test_legacy_response_fields_still_present():
    """Ensure we don't break existing clients."""
    response = client.get("/api/users")
    assert "users" in response  # Original field
    # New field is optional for old clients
```

## Contract Change Checklist

| Item | Status |
|------|--------|
| Contracts identified | |
| Impact assessed | |
| Interface docs updated | |
| Version bumped (if breaking) | |
| Migration notes written | |
| Compatibility tests added | |
| Downstream consumers notified | |

## Common Mistakes

1. **Silent breaking changes**: Changing types without version bump
2. **Missing deprecation**: Removing things without warning
3. **Doc drift**: Code changes but docs don't
4. **Test gaps**: No tests for contract compliance
5. **Cascading breaks**: Breaking internal contracts without checking callers

## Related Constraints
- CON-010: Interface changes need docs
- CON-011: Breaking changes need version bump
- CON-012: New interfaces need examples
