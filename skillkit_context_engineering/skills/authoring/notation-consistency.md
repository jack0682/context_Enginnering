# Skill: Notation Consistency

## Purpose
Ensure consistent use of notation, terminology, and formatting throughout a document.

## When to Use
- Writing technical documents
- Creating multi-section reports
- Collaborating with multiple authors
- Maintaining long-term documentation

## Procedure

### Step 1: Define Notation Table
At the start of writing, create a notation reference:

```markdown
## Notation Reference

| Symbol/Term | Meaning | First Use |
|-------------|---------|-----------|
| $\theta$ | Model parameters | Section 2.1 |
| $\mathcal{L}$ | Loss function | Section 2.2 |
| API | Application Programming Interface | Section 1 |
| "the system" | Our proposed architecture | Throughout |
```

### Step 2: First-Use Rule
On first use of any notation:
- Define it explicitly
- Add to notation table
- Use consistently thereafter

```markdown
❌ Bad: "We minimize θ using gradient descent."

✅ Good: "We minimize the loss with respect to model parameters θ ∈ ℝⁿ 
   using gradient descent."
```

### Step 3: Consistency Check
Before finalizing, check:
- [ ] Same thing always called the same name
- [ ] Same symbol always means the same thing
- [ ] No undeclared abbreviations
- [ ] Formatting is consistent (bold, italic, code)

### Step 4: Cross-Reference Glossary
Ensure alignment with `spec/03_glossary.md`:
- New terms → add to glossary
- Existing terms → use glossary definition exactly

## Formatting Standards

| Element | Format | Example |
|---------|--------|---------|
| File paths | `code` | `src/main.py` |
| Commands | `code` | `npm install` |
| Variables | `code` or *italic* | `user_id` or *n* |
| Emphasis | **bold** | **important** |
| Terms (first use) | **bold** then normal | **API** (first), API (after) |
| Math | LaTeX or Unicode | θ, ∑, ∈ |

## Common Mistakes

1. **Synonym drift**: Using "module", "component", "unit" interchangeably
2. **Symbol overload**: Using `n` for multiple different things
3. **Inconsistent casing**: "API" vs "api" vs "Api"
4. **Missing definitions**: Using abbreviations without expansion
5. **Format inconsistency**: Sometimes `path`, sometimes *path*

## Notation Audit Checklist

| Check | Pass? |
|-------|-------|
| All abbreviations expanded on first use | |
| All symbols defined | |
| No synonym drift | |
| Formatting consistent | |
| Aligned with glossary | |

## Related Constraints
- CON-030: No undefined terms
