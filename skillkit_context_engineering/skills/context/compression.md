# Skill: Context Compression

## Purpose
Reduce context size while preserving essential information for AI agents.

## When to Use
- Large codebase exceeds context window
- Many files need to be referenced
- Historical context is needed but verbose
- Preparing efficient prompts

## Procedure

### Step 1: Identify Essential Information
Categorize information by importance:

| Priority | Include | Example |
|----------|---------|---------|
| P0 - Critical | Always | Current task, constraints, interfaces |
| P1 - High | Usually | Recent changes, related modules |
| P2 - Medium | If space | Historical context, examples |
| P3 - Low | Omit | Unrelated code, old logs |

### Step 2: Apply Compression Techniques

**Technique 1: Summarization**
```markdown
# Before (500 lines of code)
def process_user(...): ...
def validate_user(...): ...
def create_user(...): ...
# ... 50 more functions

# After (summary)
## User Module (`src/user.py`)
- 53 functions for user CRUD operations
- Key functions: `create_user()`, `validate_user()`, `get_user_by_id()`
- Dependencies: `database.py`, `auth.py`
```

**Technique 2: Interface-Only View**
```python
# Before: Full implementation
def calculate_score(data: Dict, weights: List[float]) -> float:
    """Calculate weighted score from data."""
    total = 0.0
    for key, weight in zip(data.keys(), weights):
        if key in data:
            total += data[key] * weight
    return total / sum(weights)

# After: Interface only
def calculate_score(data: Dict, weights: List[float]) -> float:
    """Calculate weighted score from data."""
    ...
```

**Technique 3: Diff-Centric**
```markdown
# Instead of full file, show only changes:
## Changes to `config.py`
- Added: `MAX_RETRIES = 3`
- Modified: `TIMEOUT` from `30` to `60`
```

**Technique 4: Tree + Highlights**
```markdown
## Project Structure
src/
├── api/        # REST endpoints (focus: auth.py)
├── core/       # Business logic
├── db/         # Database (unchanged)
└── utils/      # Helpers (unchanged)

## Focus Files
- `src/api/auth.py`: Current work
- `src/core/user.py`: Related module
```

### Step 3: Create Layered Context
Provide context in layers that can be expanded:

```markdown
## Layer 0: Immediate (always include)
- Current file being edited
- Direct dependencies

## Layer 1: Related (include if asked)
- Sibling files in same module
- Recent changes

## Layer 2: Background (summarize only)
- Project structure
- Historical decisions
```

### Step 4: Validate Compression
After compressing:
- [ ] Essential information preserved
- [ ] No critical context lost
- [ ] Agent can still complete task
- [ ] Compression ratio acceptable (target: 5-10x)

## Compression Checklist

| Item | Status |
|------|--------|
| Information prioritized | |
| Appropriate technique applied | |
| Layers defined | |
| Essential context preserved | |
| Size within limits | |

## Common Mistakes

1. **Over-compression**: Losing critical details
2. **Under-compression**: Still too large for context
3. **Wrong priorities**: Keeping noise, losing signal
4. **No layers**: All-or-nothing approach
5. **Stale summaries**: Summaries don't match current code

## Related Skills
- `context/conflict-resolution.md`: When compressed info conflicts
