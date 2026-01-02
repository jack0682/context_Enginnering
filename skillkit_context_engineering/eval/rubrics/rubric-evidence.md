# Rubric: Evidence Quality

> **Purpose**: Score the quality of evidence used to support claims.
> **Usage**: Apply this rubric to `gemini_analysis.md`, `codex_plan.md`, and `claude_impl.md`.
> **Threshold**: Total score must be ≥ 8/12 to PASS.

---

## How to Use This Rubric

1. Open the target document(s)
2. For each criterion below, find relevant content and score 0-3
3. Sum the scores
4. Compare to threshold (8/12)
5. Record in `reports/latest/gate_semantic.json`

---

## Scoring Scale

| Score | Label | Meaning |
|-------|-------|---------|
| 0 | Missing | No evidence whatsoever |
| 1 | Weak | Evidence exists but indirect, vague, or inaccessible |
| 2 | Adequate | Evidence present, relevant, and accessible |
| 3 | Strong | Evidence comprehensive, verifiable, and inline |

---

## Criterion 1: Existence (0-3)

**Question**: Does evidence exist for claims made?

| Score | Indicators | Examples |
|-------|------------|----------|
| 0 | No evidence cited | "Performance improved significantly." (no data) |
| 1 | Evidence mentioned but inaccessible | "See benchmark results." (no file path) |
| 2 | Evidence file/link exists and accessible | "See `reports/bench.json` (attached)" |
| 3 | Evidence inline or immediately verifiable | "Latency: 45ms (from `time.perf_counter()` in test output above)" |

**How to Check**:
```bash
# Find claims in the document
grep -n -E "[0-9]+%|improved|faster|better|reduced" [target_file]

# For each claim found, verify there's a source
# Source patterns: "see [file]", "(from [source])", "[citation]"
```

**Scoring Guide**:
- Count total claims with numeric/comparative assertions
- Count claims with valid evidence
- Score = (claims with evidence / total claims) mapped to 0-3

---

## Criterion 2: Relevance (0-3)

**Question**: Does the evidence directly support the specific claim?

| Score | Indicators | Examples |
|-------|------------|----------|
| 0 | Evidence unrelated | Citing memory usage to support speed claim |
| 1 | Evidence tangentially related | Citing 2024 benchmark for 2026 comparison |
| 2 | Evidence directly supports | Citing before/after test for improvement claim |
| 3 | Evidence conclusively proves | Full test output showing exact values |

**How to Check**:
```markdown
For each claim-evidence pair, ask:
1. Does the evidence measure what the claim asserts?
2. Are the conditions in evidence same as claim context?
3. Is there a clear logical link?
```

**Scoring Guide**:
- 3: Evidence and claim have identical subject/context
- 2: Evidence and claim have same subject, similar context
- 1: Evidence kind-of relates but requires inference
- 0: Evidence doesn't logically connect

---

## Criterion 3: Freshness (0-3)

**Question**: Is the evidence current and applicable?

| Score | Indicators | Examples |
|-------|------------|----------|
| 0 | No timestamp or clearly outdated | "Based on 2020 benchmarks" |
| 1 | From previous version | "Tests ran on v1.0, we're on v2.0" |
| 2 | From current version | "Tests from current commit abc123" |
| 3 | Generated in this cycle | "Output from command run above:" |

**How to Check**:
```bash
# Find evidence files and check modification time
ls -la [evidence_file]

# Compare to run timestamp
cat skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md | grep Created
```

**Scoring Guide**:
- 3: Evidence generated during this run
- 2: Evidence from same day/week, same code version
- 1: Evidence from previous run or version
- 0: Evidence age unknown or demonstrably stale

---

## Criterion 4: Reproducibility (0-3)

**Question**: Can someone else generate this evidence?

| Score | Indicators | Examples |
|-------|------------|----------|
| 0 | No way to reproduce | "I tested it and it works" |
| 1 | Requires special access | "See internal dashboard (login required)" |
| 2 | Reproducible with documented steps | "Run `pytest -k test_perf`" |
| 3 | Automated reproduction | "CI generates this on each commit" |

**How to Check**:
```markdown
For each evidence item:
1. Is there a command to generate it? (2+)
2. Are dependencies/environment specified? (adds to 3)
3. Does the document include the actual output? (adds to 3)
```

**Scoring Guide**:
- 3: Command + environment + expected output all documented
- 2: Command documented, can be re-run
- 1: Procedure exists but requires setup/access
- 0: No procedure, cannot reproduce

---

## Scoring Worksheet

Use this template to score a document:

```markdown
## Evidence Rubric Scoring: [Document Name]

**Evaluator**: [Name/Agent]
**Date**: [YYYY-MM-DD]

### Claims Identified
1. [Claim 1]: "[quote]"
   - Evidence: [what supports it]
   - Location: [line/section]

2. [Claim 2]: "[quote]"
   - Evidence: [what supports it]
   - Location: [line/section]

### Criterion Scores

| Criterion | Score | Justification |
|-----------|-------|---------------|
| Existence | /3 | [why this score] |
| Relevance | /3 | [why this score] |
| Freshness | /3 | [why this score] |
| Reproducibility | /3 | [why this score] |

**Total**: X/12
**Threshold**: 8/12
**Result**: PASS / FAIL
```

---

## Complete Scoring Example

**Document**: `claude_impl.md`

**Claim Found** (line 45): "Latency reduced from 150ms to 45ms (70% improvement)"

**Evidence Provided**:
```
$ pytest tests/test_performance.py -v
test_performance.py::test_old_latency: 152ms
test_performance.py::test_new_latency: 44ms
```

**Scoring**:
| Criterion | Score | Justification |
|-----------|-------|---------------|
| Existence | 3 | Exact numbers with test output inline |
| Relevance | 3 | Test output directly measures latency claim |
| Freshness | 3 | Output from this run (shown in doc) |
| Reproducibility | 3 | Command provided, can re-run pytest |

**Total**: 12/12 — **PASS** (Strong)

---

## Threshold Table

| Total Score | Rating | Action |
|-------------|--------|--------|
| 0-3 | Fail | Reject output, re-run agent with evidence requirements |
| 4-7 | Weak | Document deficiencies, may proceed with plan to improve |
| 8-10 | Adequate | PASS — acceptable quality |
| 11-12 | Strong | Excellent — exemplary evidence |

---

## Related Constraints

- **CON-001**: Numeric claims need sources (BLOCKER)
- **CON-002**: Factual claims need citations (BLOCKER)
- **CON-003**: Visual claims need screenshots (MAJOR)

---

## Recording Results

Save rubric results to:
```
skillkit_context_engineering/reports/latest/gate_semantic.json
```

Format:
```json
{
  "rubrics": {
    "evidence": {
      "document": "claude_impl.md",
      "scores": {
        "existence": 3,
        "relevance": 3,
        "freshness": 3,
        "reproducibility": 3
      },
      "total": 12,
      "threshold": 8,
      "status": "PASS"
    }
  }
}
```

---

**Version**: 1.0.0
