# Rubric: Reproducibility Quality

> **Purpose**: Score how reproducible an action, experiment, or result is.
> **Usage**: Apply to `claude_impl.md` primarily, and `codex_plan.md` for planned builds.
> **Threshold**: Total score must be ≥ 8/12 to PASS.

---

## How to Use This Rubric

1. Identify all actions/experiments described in the document
2. For each action, evaluate the four criteria below
3. Average or take the minimum score across actions
4. Compare total to threshold (8/12)

---

## Scoring Scale

| Score | Label | Meaning |
|-------|-------|---------|
| 0 | Not reproducible | Cannot be repeated by anyone else |
| 1 | Difficult | Major effort required (special access, setup) |
| 2 | Moderate | Reproducible with documented steps |
| 3 | Easy | Fully automated or trivial to reproduce |

---

## Criterion 1: Commands (0-3)

**Question**: Are all commands documented?

| Score | Indicators | What to Look For |
|-------|------------|------------------|
| 0 | No commands shown | "I set up the database" (no `CREATE TABLE`) |
| 1 | Partial commands | "Run migrations" (which migration? what command?) |
| 2 | Complete commands | `python manage.py migrate --database=primary` |
| 3 | Script/automation | "Run `./scripts/setup.sh` which does X, Y, Z" |

**How to Check**:
```bash
# Count command blocks in claude_impl.md
grep -c '^\$\|^```bash' skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md

# Count vague action descriptions
grep -c -i "manually\|by hand\|set up\|configured" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md
```

**Scoring Decision Tree**:
```
Are commands documented?
├─ No → Score 0
└─ Yes
   ├─ Missing arguments/paths? → Score 1
   └─ Complete with arguments
      ├─ Scattered in text? → Score 2
      └─ In script/automation? → Score 3
```

---

## Criterion 2: Environment (0-3)

**Question**: Is the environment specified?

| Score | Indicators | What to Look For |
|-------|------------|------------------|
| 0 | No environment info | No mention of OS, versions, dependencies |
| 1 | OS mentioned only | "On Linux..." |
| 2 | OS + key dependencies | "Python 3.10, Ubuntu 22.04, pytest 7.0" |
| 3 | Full specification | Dockerfile, lockfile, or complete env dump |

**How to Check**:
```bash
# Look for environment specification
grep -i -E "python|node|version|ubuntu|macos|docker" \
  skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md

# Check for lockfiles in evidence
ls skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/ | \
  grep -E "requirements.*\.txt|package-lock|Pipfile\.lock|poetry\.lock"
```

**Environment Specification Levels**:
```
Level 0: Nothing
Level 1: "Python 3.10 on macOS"
Level 2: "Python 3.10.4, macOS 13.0, pytest 7.4.0, fastapi 0.100.0"
Level 3: requirements.txt attached + Python version + OS version
         OR Dockerfile provided
         OR `pip freeze > requirements.txt` output saved
```

---

## Criterion 3: Data (0-3)

**Question**: Is input data available?

| Score | Indicators | What to Look For |
|-------|------------|------------------|
| 0 | Data not provided | "Using the training dataset" (which one?) |
| 1 | Described but inaccessible | "Dataset from Company X internal DB" |
| 2 | Available | "Download from [URL]" or path to file |
| 3 | Versioned + checksummed | "v1.2.0, sha256: abc123..." |

**How to Check**:
```bash
# Look for data references
grep -i -E "dataset|data|input|file.*\.(csv|json|txt|db)" \
  skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md

# For each reference, verify accessibility
# - If URL: can you access it?
# - If path: does file exist?
# - If internal: is there a download procedure?
```

**Data Accessibility Checklist**:
- [ ] Data source is identified
- [ ] Download/access method is documented
- [ ] File format is specified
- [ ] (Bonus) Checksum or version provided
- [ ] (Bonus) Sample data included

---

## Criterion 4: Randomness (0-3)

**Question**: Is randomness controlled?

| Score | Indicators | What to Look For |
|-------|------------|------------------|
| 0 | Random ops, no seed | `random.shuffle(data)` with no seed |
| 1 | Seed mentioned but unverified | "We used seed 42" (not confirmed in code) |
| 2 | Fixed seeds documented | `random.seed(42); np.random.seed(42)` |
| 3 | Deterministic or verified | CUDA deterministic mode + seeds logged |

**How to Check**:
```bash
# Scan code for random operations without seeds
grep -rn "random\." --include="*.py" src/ | grep -v "seed"

# Look for seed setting
grep -rn "seed" --include="*.py" src/ | head -10
```

**Randomness Sources to Check**:
- `random` module: `random.seed()`
- `numpy`: `np.random.seed()`
- `torch`: `torch.manual_seed()`, `torch.cuda.manual_seed_all()`
- Data shuffling: Is order reproducible?
- Initialization: Weight init with seeds?

**N/A Case**: If no random operations, score 3 (deterministic by default).

---

## Scoring Worksheet

```markdown
## Reproducibility Rubric Scoring: [Document Name]

**Evaluator**: [Name/Agent]
**Date**: [YYYY-MM-DD]

### Actions Identified
1. [Action 1]: "[brief description]"
   - Command provided: [Yes/No]
   - Environment: [specified/missing]
   
2. [Action 2]: "[brief description]"
   - Command provided: [Yes/No]
   - Environment: [specified/missing]

### Criterion Scores

| Criterion | Score | Justification |
|-----------|-------|---------------|
| Commands | /3 | [list of commands found, or gaps] |
| Environment | /3 | [what's specified] |
| Data | /3 | [data sources and accessibility] |
| Randomness | /3 | [seeds found or N/A] |

**Total**: X/12
**Threshold**: 8/12
**Result**: PASS / FAIL

### Improvement Recommendations
- [ ] [What would bring score up]
```

---

## Complete Scoring Example

**Document**: `claude_impl.md` for ML training task

**Actions Found**:
1. Train model: `python train.py --epochs 10`
2. Evaluate: `python eval.py`
3. Dataset preparation: (described but no command)

**Scoring**:

| Criterion | Score | Justification |
|-----------|-------|---------------|
| Commands | 2 | `train.py` and `eval.py` with args, but dataset prep is vague |
| Environment | 2 | "Python 3.10, PyTorch 2.0" mentioned, no lockfile |
| Data | 1 | "Use MNIST" but no download command |
| Randomness | 3 | `torch.manual_seed(42)` in train.py line 5 |

**Total**: 8/12 — **PASS** (Adequate)

**Recommendations**:
- Add `torchvision.datasets.MNIST(download=True)` command
- Include `pip freeze > requirements.txt` output

---

## Reproduction Test Protocol

To verify reproducibility, attempt:

```bash
# 1. Fresh environment
python -m venv test_repro
source test_repro/bin/activate

# 2. Install dependencies (from documented sources)
pip install -r requirements.txt  # Or documented install commands

# 3. Get data (from documented sources)
# [Run documented data download command]

# 4. Run documented commands
# [Copy-paste each command from the document]

# 5. Compare outputs
diff expected_output.txt actual_output.txt
```

If any step fails: deduct points from relevant criterion.

---

## Threshold Table

| Total Score | Rating | Action |
|-------------|--------|--------|
| 0-3 | Fail | Not reproducible, require rewrite |
| 4-7 | Weak | Document gaps, may proceed with caveats |
| 8-10 | Adequate | PASS — can be reproduced with effort |
| 11-12 | Strong | Excellent — trivially reproducible |

---

## Recording Results

```json
{
  "rubrics": {
    "repro": {
      "document": "claude_impl.md",
      "scores": {
        "commands": 2,
        "environment": 2,
        "data": 1,
        "randomness": 3
      },
      "total": 8,
      "threshold": 8,
      "status": "PASS",
      "notes": "Needs dataset download command for score improvement"
    }
  }
}
```

---

## Related Constraints

- **CON-020**: Actions must be repeatable commands (BLOCKER)
- **CON-021**: Environment must be documented (MAJOR)
- **CON-022**: Random ops need seeds (MINOR)

---

**Version**: 1.0.0
