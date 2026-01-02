# Skill: Evidence-First Writing

## Purpose
Ensure every claim in written documentation is supported by verifiable evidence.

## When to Use
- Writing technical documentation
- Creating reports
- Documenting experimental results
- Writing papers or proposals

## Procedure

### Step 1: Identify Claims
Before writing, list all claims you will make:
- [ ] Quantitative claims (numbers, metrics, percentages)
- [ ] Comparative claims ("faster", "better", "more efficient")
- [ ] Factual claims (how something works, what something does)
- [ ] Causal claims (X causes Y)

### Step 2: Prepare Evidence
For each claim, prepare evidence BEFORE writing:

| Claim Type | Evidence Required |
|------------|-------------------|
| Quantitative | Command output, data file, benchmark result |
| Comparative | Before/after measurements, baseline comparison |
| Factual | Documentation cite, source code reference |
| Causal | Controlled experiment, isolation test |

### Step 3: Write with Citations
Write each claim with inline evidence references:

```markdown
❌ Bad: "Performance improved significantly."

✅ Good: "Latency decreased from 150ms to 45ms (70% reduction), 
   measured using `bench.py` on commit abc123 
   (see: reports/benchmark_2026-01-02.json)."
```

### Step 4: Verify Evidence Exists
Before submitting:
- [ ] Every quantitative claim has a data source file
- [ ] Every file referenced actually exists
- [ ] Commands can be re-run to reproduce data

## Evidence Checklist

| Category | Question | Pass? |
|----------|----------|-------|
| Existence | Does the evidence file exist? | |
| Relevance | Does the evidence support this specific claim? | |
| Freshness | Is the evidence from this run/version? | |
| Reproducibility | Can someone else generate this evidence? | |

## Common Mistakes

1. **Vague quantifiers**: "much faster" → use exact numbers
2. **Missing baseline**: "improved X" → improved from what?
3. **Stale evidence**: citing old benchmarks for current code
4. **Broken references**: evidence file doesn't exist or was moved
5. **Aggregation without raw data**: only showing averages, not raw measurements

## Related Constraints
- CON-001: Numeric claims need sources
- CON-002: Factual claims need citations
