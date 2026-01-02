# Checkpoints Directory

> **Purpose**: Record failure points, rollback commands, and recovery notes.

## Usage

When a gate fails or you need to rollback:

1. Create a checkpoint file: `YYYY-MM-DD__description.md`
2. Document:
   - What failed
   - What was the state
   - How to recover/rollback
   - Lessons learned

## Template

```markdown
# Checkpoint: [Date] [Description]

## Trigger
What caused this checkpoint?

## State at Failure
- Commit: `abc123`
- Files changed: [list]
- Gate failed: [which gate]

## Rollback Commands
```bash
git checkout HEAD -- file1 file2
```

## Recovery Steps
1. ...
2. ...

## Lessons Learned
- ...

## Resolution
- [ ] Rollback completed
- [ ] Issue fixed
- [ ] Prevention added to checklists
```

---

## Checkpoint Log

| Date | Description | Status |
|------|-------------|--------|
| | | |
