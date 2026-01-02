# Skill: Citation Hygiene

> **Purpose**: Maintain accurate, consistent, and verifiable citations in all documents.
> **When to Use**: Writing papers, reports, documentation, or any claim about external work.
> **Related Constraints**: CON-002 (Factual claims need citations)

---

## Overview

Good citation hygiene ensures:
1. **Verifiability**: Anyone can check your sources
2. **Reproducibility**: Future you (or others) can find the same information
3. **Credibility**: Your work is grounded in established knowledge
4. **Legal safety**: Proper attribution avoids plagiarism

---

## Step-by-Step Procedure

### Step 1: Capture Complete Citation Data

**When you encounter a source**, immediately record:

```yaml
# citations.yaml (keep one per project)
sources:
  - id: SMITH2025          # Short ID for inline reference
    type: paper            # paper / book / doc / website / code
    title: "A Novel Approach to Authentication"
    authors:
      - "Smith, John"
      - "Doe, Alice"
    year: 2025
    venue: "ACM Conference on Security"
    url: "https://doi.org/10.1234/example"
    doi: "10.1234/example"
    accessed: "2026-01-02"
    local_copy: "refs/smith2025.pdf"  # If you saved it
    notes: "See section 4.2 for auth protocol"
```

**Minimum required fields by type**:

| Type | Required Fields |
|------|-----------------|
| Paper | title, authors, year, venue, doi/url |
| Book | title, authors, year, publisher, isbn |
| Documentation | title, version, url, accessed |
| Website | title, url, accessed, archived_url |
| Code | name, version, url, license |

### Step 2: Choose and Apply Consistent Format

**For Technical Documentation** (recommended):
```markdown
According to Smith et al. [SMITH2025], the standard approach uses...

## References
[SMITH2025] Smith, J., Doe, A. (2025). A Novel Approach to Authentication. 
            ACM Conference on Security. https://doi.org/10.1234/example
```

**For Academic Papers (APA-ish)**:
```markdown
The approach was validated (Smith & Doe, 2025) using standard benchmarks.

## References
Smith, J., & Doe, A. (2025). A Novel Approach to Authentication. 
In Proceedings of ACM Conference on Security. 
https://doi.org/10.1234/example
```

**For Code/Library References**:
```markdown
Built using FastAPI [FASTAPI] for the web framework.

## Dependencies
[FASTAPI] FastAPI v0.100.0. https://fastapi.tiangolo.com. 
          License: MIT. Accessed: 2026-01-02.
```

### Step 3: Verify Links Before Publishing

**Automated check**:
```bash
#!/bin/bash
# check_citations.sh — Run before publishing

echo "Checking citation URLs..."

# Extract URLs from markdown
grep -ohE "https?://[^)]+" document.md | while read url; do
  status=$(curl -s -o /dev/null -w "%{http_code}" "$url" --max-time 5)
  if [ "$status" != "200" ]; then
    echo "⚠️  BROKEN ($status): $url"
  else
    echo "✓ OK: $url"
  fi
done
```

**Manual verification checklist**:
- [ ] All URLs return 200 (or valid redirect)
- [ ] DOIs resolve correctly
- [ ] Archived links (archive.org) for volatile sources
- [ ] Version numbers match cited content

### Step 4: Handle Edge Cases

**When source has no author**:
```markdown
[ORG2025] Organization Name. (2025). Document Title. URL.
```

**When source has no date**:
```markdown
[SOURCE-ND] Author. (n.d.). Title. Retrieved 2026-01-02 from URL.
```

**When citing specific section/page**:
```markdown
As noted by Smith [SMITH2025, §4.2], the algorithm requires...
OR
As noted by Smith (2025, p. 145), the algorithm requires...
```

**When citing private/internal sources**:
```markdown
Internal analysis [INTERNAL-001] showed similar results.

## References
[INTERNAL-001] Internal Analysis Report. 2025-06-15. 
               Path: //internal/docs/analysis.pdf. Access: Team only.
               Note: Request access from security@company.com
```

### Step 5: Maintain Citation Database

**Keep citations in a single YAML file**:
```bash
# Location
skillkit_context_engineering/refs/citations.yaml

# Or project-specific
project_root/docs/citations.yaml
```

**Add new citations incrementally**:
```yaml
# When you find a new source:
# 1. Open citations.yaml
# 2. Add entry with unique ID
# 3. Use ID in document immediately
# 4. Fill in full details when you have time
```

**Partial entry (acceptable temporarily)**:
```yaml
- id: TEMP-PAPER-X
  title: "That paper about X"
  url: "https://..."
  status: INCOMPLETE  # Flag for later completion
  notes: "Found during research, need full details"
```

---

## Citation Quality Checklist

Use before finalizing any document:

```markdown
## Citation Hygiene Checklist

### Completeness
- [ ] Every factual claim has a citation
- [ ] Every number/statistic has a source
- [ ] Every quote is attributed
- [ ] All citations have complete information

### Accuracy
- [ ] Author names spelled correctly
- [ ] Years are correct
- [ ] URLs are working
- [ ] Versions match cited APIs/libraries

### Consistency
- [ ] Format is consistent throughout
- [ ] Same source uses same citation ID
- [ ] Reference list is alphabetized (or numbered in order)

### Accessibility
- [ ] Public sources preferred where possible
- [ ] Archived links for volatile web pages
- [ ] Local copies saved for critical sources
```

---

## Common Mistakes & Fixes

| Mistake | Example | Fix |
|---------|---------|-----|
| Broken link | `[1] https://expired-url.com` | Use archive.org, update URL |
| Missing year | `Smith et al. say...` | Add year: `Smith et al. (2025)` |
| Vague source | `According to research...` | Be specific: `According to [CITE]` |
| Version missing | `Using TensorFlow` | `Using TensorFlow 2.12.0 [TF212]` |
| Secondary citation | `Smith (2020) cited by Jones (2022)` | Cite original if possible |
| Self-plagiarism | Reusing own text without cite | Cite your own previous work |

---

## Citation Format Templates

### For Papers
```markdown
[ID] Last, F., Last, F. (YYYY). Title of Paper. 
     Conference/Journal Name. DOI/URL.
```

### For Books
```markdown
[ID] Last, F. (YYYY). Title of Book (Edition). 
     Publisher. ISBN.
```

### For Documentation
```markdown
[ID] Product Name Documentation vX.Y. URL. 
     Accessed: YYYY-MM-DD.
```

### For Code Libraries
```markdown
[ID] Library Name vX.Y.Z. URL. 
     License: LICENSE_TYPE. Accessed: YYYY-MM-DD.
```

### For Web Pages
```markdown
[ID] Page Title. Website Name. URL. 
     Published: YYYY-MM-DD. Accessed: YYYY-MM-DD.
     Archived: https://web.archive.org/...
```

### For Internal Documents
```markdown
[ID] Document Title. Organization. YYYY-MM-DD.
     Path: internal/path/to/doc.pdf.
     Access: [who can access].
```

---

## Tooling Suggestions

**For managing citations**:
- Zotero (free, open source)
- Mendeley (free for basic use)
- BibTeX (for LaTeX users)
- Simple YAML file (for small projects)

**For checking citations**:
```bash
# Find potential uncited claims
grep -n -E "(research shows|studies indicate|according to|evidence suggests)" doc.md

# Find citation markers
grep -n -E "\[[A-Z]+[0-9]+\]|\([A-Za-z]+, [0-9]+\)" doc.md

# Compare to find gaps
```

**For archiving links**:
- https://web.archive.org/save/ — Save any URL
- https://archive.ph — Alternative archiver
- Local PDF save — For critical sources

---

## Evidence of Good Citation Hygiene

When reviewing, look for:

✅ **Good**:
```markdown
According to Smith et al. [SMITH2025], the algorithm achieves 
95% accuracy on the MNIST dataset (see their Table 3).
```

❌ **Bad**:
```markdown
Research shows the algorithm works well.
```

✅ **Good**:
```markdown
Built with FastAPI v0.100.0 [FASTAPI] and Pydantic v2.0 [PYDANTIC].

## References
[FASTAPI] FastAPI v0.100.0. https://fastapi.tiangolo.com. MIT License.
[PYDANTIC] Pydantic v2.0. https://docs.pydantic.dev. MIT License.
```

❌ **Bad**:
```markdown
Built with FastAPI and Pydantic.
```

---

## Quick Reference

```
╔═══════════════════════════════════════════════════════════╗
║  CITATION HYGIENE QUICK REFERENCE                         ║
╠═══════════════════════════════════════════════════════════╣
║                                                           ║
║  CAPTURE                                                  ║
║  ───────                                                  ║
║  ID + title + authors + year + url + accessed             ║
║                                                           ║
║  FORMAT                                                   ║
║  ──────                                                   ║
║  Pick one: [ID], (Author, Year), or numbered [1]          ║
║  Be consistent throughout document                        ║
║                                                           ║
║  VERIFY                                                   ║
║  ──────                                                   ║
║  Check URLs (curl -s -o /dev/null -w "%{http_code}" URL)  ║
║  Archive volatile pages                                   ║
║  Verify versions match                                    ║
║                                                           ║
║  MINIMUM                                                  ║
║  ───────                                                  ║
║  Every "X says", "research shows", number → needs cite    ║
║  Every library/tool used → needs version + cite           ║
║                                                           ║
╚═══════════════════════════════════════════════════════════╝
```

---

**Version**: 1.0.0
