# Rubric: Logic Quality

> **Purpose**: Score the logical soundness of reasoning and arguments.

---

## Scoring Scale

| Score | Label | Description |
|-------|-------|-------------|
| 0 | Invalid | Logical errors present |
| 1 | Weak | Reasoning is unclear or has gaps |
| 2 | Adequate | Reasoning is sound but not rigorous |
| 3 | Strong | Reasoning is rigorous and complete |

---

## Evaluation Criteria

### Criterion 1: Definitions
*Are all terms and symbols defined?*

| Score | Indicator |
|-------|-----------|
| 0 | Undefined terms used without explanation |
| 1 | Some terms defined, others assumed |
| 2 | All terms defined or in glossary |
| 3 | Precise definitions with examples |

### Criterion 2: Consistency
*Is the reasoning internally consistent?*

| Score | Indicator |
|-------|-----------|
| 0 | Direct contradictions present |
| 1 | Implicit tensions or ambiguities |
| 2 | No contradictions, some ambiguity |
| 3 | Fully consistent, explicit assumptions |

### Criterion 3: Completeness
*Are all steps of reasoning present?*

| Score | Indicator |
|-------|-----------|
| 0 | Major logical jumps or missing steps |
| 1 | Some steps implied, not stated |
| 2 | All major steps present |
| 3 | Complete chain with no gaps |

### Criterion 4: Validity
*Do conclusions follow from premises?*

| Score | Indicator |
|-------|-----------|
| 0 | Conclusions do not follow |
| 1 | Conclusions partially supported |
| 2 | Conclusions reasonably follow |
| 3 | Conclusions necessarily follow |

---

## Common Logic Errors

| Error | Description | Detection |
|-------|-------------|-----------|
| Non sequitur | Conclusion doesn't follow | Check each step |
| Circular reasoning | Assumes what it proves | Trace premise sources |
| False dichotomy | Only two options when more exist | Count alternatives |
| Hasty generalization | Conclusion from few examples | Check sample size |
| Equivocation | Term used with different meanings | Check definitions |

---

## Scoring Example

**Argument**: "Since test coverage is 80%, the code is reliable."

| Criterion | Score | Notes |
|-----------|-------|-------|
| Definitions | 1 | "Reliable" not defined |
| Consistency | 2 | No contradictions |
| Completeness | 1 | Missing: coverage ≠ reliability |
| Validity | 0 | Conclusion doesn't follow from premise |

**Total**: 4/12 = **Weak** (fails validity)

---

## Thresholds

| Total Score | Rating | Action |
|-------------|--------|--------|
| 0-3 | Fail | Logic rejected |
| 4-7 | Weak | Requires revision |
| 8-10 | Adequate | Acceptable |
| 11-12 | Strong | Rigorous |

---

## Related Constraints
- CON-030: No undefined terms
- CON-031: No contradictions
