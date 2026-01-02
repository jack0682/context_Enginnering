# Skill: Claim Traceability

## Purpose
Ensure every claim can be traced back to its origin and forward to its impact.

## When to Use
- Writing documents that will be reviewed
- Making decisions based on previous analysis
- Building on prior work
- Creating audit trails

## Procedure

### Step 1: Establish Claim Chain
For each claim, document:
1. **Source**: Where did this claim originate?
2. **Transformation**: How was the source information processed?
3. **Current statement**: What are you claiming now?
4. **Downstream impact**: What depends on this claim?

### Step 2: Use Traceable References
Format references consistently:

```markdown
## Source References
- [SRC-001] Original paper/document
- [SRC-002] Experimental data file

## Claims
- Claim 1: "X is true" [SRC-001, Section 3.2]
- Claim 2: "Y was measured as Z" [SRC-002, row 15]
```

### Step 3: Maintain Trace Links
Create explicit links:

| Claim ID | Claim | Source | Derived From | Used By |
|----------|-------|--------|--------------|---------|
| CLM-001 | Original finding | experiment.log | — | CLM-002 |
| CLM-002 | Interpreted result | analysis.py | CLM-001 | CLM-003 |
| CLM-003 | Conclusion | — | CLM-002 | Decision X |

### Step 4: Validate Chain
Before publishing:
- [ ] Every claim has a source
- [ ] Transformations are documented
- [ ] No orphan claims (claims with no source)
- [ ] No broken chains (missing intermediate steps)

## Traceability Levels

| Level | Description | Example |
|-------|-------------|---------|
| L1 - Direct | Claim quotes source directly | "Paper says X" |
| L2 - Derived | Claim is computed from source | "We calculated Y from data" |
| L3 - Interpreted | Claim requires judgment | "This suggests Z" |
| L4 - Inferred | Claim is extrapolated | "Therefore, W might be true" |

Higher levels require more explicit justification.

## Common Mistakes

1. **Orphan claims**: Conclusions with no source
2. **Broken chains**: Intermediate claims missing
3. **Level confusion**: Treating L4 (inferred) as L1 (direct)
4. **Stale traces**: Sources that no longer exist or have changed

## Related Constraints
- CON-002: Factual claims need citations
- CON-031: No contradictions
