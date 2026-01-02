# Skill: Conflict Resolution

## Purpose
Resolve conflicts between different sources of information or instructions.

## When to Use
- Multiple documents say different things
- Current code contradicts documentation
- User instructions conflict with specs
- Historical decisions conflict with new requirements

## Procedure

### Step 1: Identify the Conflict
Document the conflict explicitly:

```markdown
## Conflict Record

**Source A**: spec/01_constraints.md, line 45
**Says**: "All functions must have docstrings"

**Source B**: User instruction (2026-01-02)
**Says**: "Just write the code quickly, skip docs"

**Conflict Type**: SPEC vs USER
```

### Step 2: Apply Authority Hierarchy
Resolve based on source authority:

| Priority | Source | Example |
|----------|--------|---------|
| 1 (Highest) | Safety constraints | "Never store plaintext passwords" |
| 2 | Spec (`spec/*.md`) | "All functions need docstrings" |
| 3 | Checklist items | "Run tests before merge" |
| 4 | Casefile/User instruction | "Skip docs for now" |
| 5 (Lowest) | Agent judgment | "I think we should..." |

**Resolution for example above**:
Spec wins. Write docstrings despite user's request for speed.

### Step 3: Document Resolution
Record the decision:

```markdown
## Resolution

**Decision**: Follow spec (source A)
**Rationale**: Spec has higher authority than ad-hoc user instructions
**Action**: Write docstrings as required
**Communicated to user**: Yes (in impl log)

**If user disagrees**: They must update the spec via change policy
```

### Step 4: Escalate if Needed
Some conflicts require human decision:

| Conflict Type | Action |
|---------------|--------|
| Safety vs anything | Safety wins, no escalation |
| Spec vs Spec | Escalate to architect |
| User vs Spec | Document, follow spec, notify user |
| Code vs Docs | Fix whichever is wrong |
| Ambiguous | Escalate with options |

## Conflict Types

### Type 1: Contradiction
Two sources say opposite things.
→ Apply authority hierarchy.

### Type 2: Ambiguity
One source is unclear.
→ Check glossary, ask for clarification.

### Type 3: Omission
Expected guidance missing.
→ Apply safest default, document gap.

### Type 4: Staleness
Information is outdated.
→ Trust newer source, update older source.

## Conflict Resolution Template

```markdown
## Conflict: [Short title]

### Sources
- A: [location] says [X]
- B: [location] says [Y]

### Type
[Contradiction / Ambiguity / Omission / Staleness]

### Resolution
- Winner: [A / B / Neither]
- Rationale: [why]
- Action: [what to do]
- Follow-up: [update docs? escalate?]
```

## Checklist

| Item | Status |
|------|--------|
| Conflict identified explicitly | |
| Sources documented | |
| Authority hierarchy applied | |
| Resolution recorded | |
| Necessary updates made | |
| User notified if needed | |

## Common Mistakes

1. **Silent resolution**: Picking one without documenting
2. **Wrong hierarchy**: User instruction overrides spec
3. **No follow-up**: Leaving conflicting docs unfixed
4. **Escalation avoidance**: Guessing instead of asking
5. **Authority confusion**: Not knowing what trumps what

## Related Constraints
- CON-031: No contradictions
- CON-042: Spec changes need approval
