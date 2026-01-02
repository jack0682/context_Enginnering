# Rubric: Claims Quality

> **Purpose**: Score the quality and traceability of claims made in documents.

---

## Scoring Scale

| Score | Label | Description |
|-------|-------|-------------|
| 0 | Unsubstantiated | Claim has no support |
| 1 | Weak | Claim has weak or indirect support |
| 2 | Adequate | Claim is supported and traceable |
| 3 | Strong | Claim is fully traced with clear provenance |

---

## Evaluation Criteria

### Criterion 1: Traceability
*Can the claim be traced to its origin?*

| Score | Indicator |
|-------|-----------|
| 0 | No source indicated |
| 1 | Vague source ("papers show...") |
| 2 | Specific source cited |
| 3 | Full trace chain documented |

### Criterion 2: Precision
*Is the claim stated precisely?*

| Score | Indicator |
|-------|-----------|
| 0 | Vague ("significantly better") |
| 1 | Qualitative but bounded |
| 2 | Quantitative but rounded |
| 3 | Precise with uncertainty bounds |

### Criterion 3: Scope
*Are the claim's boundaries clear?*

| Score | Indicator |
|-------|-----------|
| 0 | Overclaims or universal statements |
| 1 | Scope implied but not stated |
| 2 | Scope stated |
| 3 | Scope, conditions, and limits explicit |

### Criterion 4: Falsifiability
*Can the claim be tested/verified?*

| Score | Indicator |
|-------|-----------|
| 0 | Cannot be tested |
| 1 | Testable with significant effort |
| 2 | Testable with available tools |
| 3 | Test/verification included |

---

## Claim Categories

| Category | Example | Key Check |
|----------|---------|-----------|
| Performance | "Faster by X%" | Benchmark present? |
| Capability | "Supports feature Y" | Demo/test present? |
| Comparison | "Better than Z" | Baseline defined? |
| Causal | "Causes improvement in W" | Controlled test? |

---

## Scoring Example

**Claim**: "Our approach is state-of-the-art on benchmark X."

| Criterion | Score | Notes |
|-----------|-------|-------|
| Traceability | 2 | Benchmark name specified |
| Precision | 1 | No metrics given |
| Scope | 1 | "State-of-the-art" is vague |
| Falsifiability | 2 | Benchmark is public, testable |

**Total**: 6/12 = **Weak**

Better version: "Our approach achieves 94.3% accuracy on benchmark X (v2.0), 
surpassing the previous best of 92.1% (Model Y, 2025). Results are reproducible 
using script `run_benchmark.py`."

---

## Thresholds

| Total Score | Rating | Action |
|-------------|--------|--------|
| 0-3 | Fail | Claim rejected |
| 4-7 | Weak | Needs improvement |
| 8-10 | Adequate | Acceptable |
| 11-12 | Strong | Excellent |

---

## Related Constraints
- CON-001: Numeric claims need sources
- CON-002: Factual claims need citations
