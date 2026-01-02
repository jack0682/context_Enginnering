# Skill: Structured Extraction

## Purpose
Extract information from sources (papers, docs, code) in a structured, reusable format.

## When to Use
- Reading research papers
- Analyzing documentation
- Reviewing codebases
- Extracting requirements

## Procedure

### Step 1: Define Extraction Schema
Before reading, define what you need:

```markdown
## Extraction Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| source | string | Yes | Where this came from |
| claim | string | Yes | The main assertion |
| evidence | string | Yes | Supporting data |
| method | string | No | How it was done |
| limitations | string | No | Stated limitations |
| relevance | 1-5 | Yes | How relevant to our work |
```

### Step 2: Read with Schema in Mind
As you read, populate the schema:

```markdown
## Extracted Items

### Item 1
- **source**: "Smith et al. 2025, Section 3.2"
- **claim**: "Method X achieves 95% accuracy on dataset Y"
- **evidence**: "Table 2, row 3"
- **method**: "Fine-tuned BERT with custom loss"
- **limitations**: "Only tested on English text"
- **relevance**: 4 (directly applicable to our NLP task)

### Item 2
...
```

### Step 3: Cross-Reference
Link extracted items to your project:

| Extracted Claim | Our Module | Impact | Action |
|-----------------|------------|--------|--------|
| "Method X achieves 95%" | `nlp/classify.py` | Could improve accuracy | Evaluate |
| "Requires GPU for training" | Infrastructure | Need GPU setup | Plan |

### Step 4: Validate Extraction
- [ ] Every claim has a source citation
- [ ] No misquotes or paraphrasing errors
- [ ] Limitations noted
- [ ] Relevance assessed honestly

## Extraction Templates

### For Research Papers
```markdown
## Paper: [Title]
**Authors**: ...
**Year**: ...
**URL/DOI**: ...

### Key Claims
1. ...

### Methodology
...

### Results
| Metric | Value | Dataset |
|--------|-------|---------|
| ... | ... | ... |

### Limitations (stated by authors)
...

### Our Assessment
- Relevance: X/5
- Applicability: ...
- Concerns: ...
```

### For Documentation
```markdown
## Doc: [Title]
**Source**: [URL]
**Version**: ...
**Last Accessed**: ...

### Key Information
...

### API/Interface Details
...

### Gotchas/Warnings
...
```

## Common Mistakes

1. **Selective extraction**: Only noting what confirms your beliefs
2. **Missing context**: Extracting claims without conditions
3. **Citation laziness**: "I read somewhere that..."
4. **Over-extraction**: Noting everything, burying the important
5. **No linkage**: Extracted info not connected to your work

## Related Constraints
- CON-002: Factual claims need citations
