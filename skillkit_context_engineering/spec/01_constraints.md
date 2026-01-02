# 01. Constraints

> **Purpose**: This document defines the MUST and MUST NOT rules that govern all work under this kit. Every constraint has a unique ID and severity tag.

---

## 1. Constraint Format

Each constraint follows this structure:

```
[CATEGORY.SUBCATEGORY.SEVERITY]
CON-XXX: Constraint statement
```

**Severity Levels**:
- **BLOCKER**: Violation stops the pipeline. Cannot proceed.
- **MAJOR**: Violation must be documented and planned for fix. Can proceed with caution.
- **MINOR**: Violation should be noted. Can proceed.

---

## 2. Evidence Constraints

### [EVIDENCE.NUMERIC.BLOCKER]
**CON-001**: All numeric claims (metrics, measurements, percentages) MUST have a source reference or be generated from recorded commands.

*Example Violation*: "Performance improved by 50%" with no benchmark data.
*Evidence Required*: Command output, log file, or data file path.

### [EVIDENCE.FACTUAL.BLOCKER]
**CON-002**: All factual claims about external systems, libraries, or APIs MUST cite the documentation version or be verified against the actual system.

*Example Violation*: "This API requires authentication" without citing docs.
*Evidence Required*: Documentation link, version, and relevant excerpt.

### [EVIDENCE.SCREENSHOT.MAJOR]
**CON-003**: UI/visual claims MUST include screenshot evidence saved to `ops/RUNS/<run>/01_inputs/`.

*Example Violation*: "The button is now blue" with no screenshot.
*Evidence Required*: Screenshot file path.

---

## 3. Contract Constraints

### [CONTRACT.INTERFACE.BLOCKER]
**CON-010**: Any change to a public interface (function signature, API endpoint, schema) MUST be documented in `spec/02_interfaces.md` BEFORE implementation.

*Example Violation*: Adding a parameter to a public function without updating interface docs.
*Evidence Required*: Updated `02_interfaces.md` with change description.

### [CONTRACT.VERSION.MAJOR]
**CON-011**: Interface changes that break backward compatibility MUST increment the MAJOR version number.

*Example Violation*: Removing a required field without version bump.
*Evidence Required*: Version update in relevant files.

### [CONTRACT.SAMPLE.MAJOR]
**CON-012**: New interfaces MUST include at least one usage example in documentation.

*Example Violation*: New API endpoint with no example request/response.
*Evidence Required*: Example in docs or `02_interfaces.md`.

---

## 4. Reproducibility Constraints

### [REPRO.COMMAND.BLOCKER]
**CON-020**: Every action that modifies state MUST be recorded as a repeatable command.

*Example Violation*: "I manually edited the config file."
*Evidence Required*: Actual command or script that performs the edit.

### [REPRO.ENV.MAJOR]
**CON-021**: Environment-dependent operations MUST document the environment (OS, versions, dependencies).

*Example Violation*: "Run `npm install`" without specifying Node version.
*Evidence Required*: Environment specification in casefile or notes.

### [REPRO.SEED.MINOR]
**CON-022**: Random operations MUST use fixed seeds or document the randomness.

*Example Violation*: Training ML model without logging seed.
*Evidence Required*: Seed value in logs or config.

---

## 5. Logic Constraints

### [LOGIC.UNDEFINED.BLOCKER]
**CON-030**: No undefined terms, symbols, or abbreviations may be used without definition in `spec/03_glossary.md`.

*Example Violation*: Using "ONN" without defining it.
*Evidence Required*: Entry in glossary.

### [LOGIC.CONTRADICTION.BLOCKER]
**CON-031**: No logical contradictions may exist within a single document or across related documents.

*Example Violation*: Spec says "X must be true" and "X must be false" simultaneously.
*Evidence Required*: Review and resolution of contradiction.

### [LOGIC.CIRCULAR.MAJOR]
**CON-032**: No circular dependencies in module design or reasoning chains.

*Example Violation*: A depends on B, B depends on A.
*Evidence Required*: Dependency graph review.

---

## 6. Change Policy Constraints

### [CHANGE.IMPACT.BLOCKER]
**CON-040**: Changes affecting more than 3 modules MUST have impact analysis in the casefile.

*Example Violation*: Large refactor with no impact documentation.
*Evidence Required*: Impact section in `00_casefile.md`.

### [CHANGE.ROLLBACK.MAJOR]
**CON-041**: Every change MUST have a documented rollback procedure.

*Example Violation*: Database migration with no rollback script.
*Evidence Required*: Rollback command or script path.

### [CHANGE.APPROVAL.MAJOR]
**CON-042**: Spec changes (`spec/*.md`) MUST be approved by the Human Architect before implementation.

*Example Violation*: Agent modifies `01_constraints.md` without approval.
*Evidence Required*: Approval note or commit with human sign-off.

---

## 7. Security Constraints

### [SECURITY.SECRETS.BLOCKER]
**CON-050**: No secrets, tokens, passwords, or API keys may appear in any file.

*Example Violation*: `api_key = "sk-xxx..."` in code.
*Evidence Required*: Grep/scan showing no secrets.

### [SECURITY.INJECTION.BLOCKER]
**CON-051**: All user inputs MUST be sanitized before use in commands, queries, or templates.

*Example Violation*: `os.system(user_input)` without validation.
*Evidence Required*: Sanitization code or validation logic.

---

## 8. Output Constraints

### [OUTPUT.PATH.BLOCKER]
**CON-060**: Agent outputs MUST be saved to the fixed paths defined in `OPERATOR_GUIDE.md`.

*Example Violation*: Gemini saves analysis to `/tmp/analysis.md`.
*Evidence Required*: Output exists at correct path.

### [OUTPUT.FORMAT.BLOCKER]
**CON-061**: Agent outputs MUST follow the format templates defined in `ops/PROMPTS/*.md`.

*Example Violation*: Claude output missing "Evidence" section.
*Evidence Required*: Output contains all required sections.

### [OUTPUT.COMPLETE.MAJOR]
**CON-062**: No output may contain TODO-only stubs without implementation.

*Example Violation*: `# TODO: implement this` with no actual code.
*Evidence Required*: Functional code or explicit justification.

---

## 9. Constraint Summary Table

| ID | Category | Severity | One-Line Summary |
|----|----------|----------|------------------|
| CON-001 | EVIDENCE | BLOCKER | Numeric claims need sources |
| CON-002 | EVIDENCE | BLOCKER | Factual claims need citations |
| CON-003 | EVIDENCE | MAJOR | Visual claims need screenshots |
| CON-010 | CONTRACT | BLOCKER | Interface changes need docs |
| CON-011 | CONTRACT | MAJOR | Breaking changes need version bump |
| CON-012 | CONTRACT | MAJOR | New interfaces need examples |
| CON-020 | REPRO | BLOCKER | Actions must be repeatable commands |
| CON-021 | REPRO | MAJOR | Environment must be documented |
| CON-022 | REPRO | MINOR | Random ops need seeds |
| CON-030 | LOGIC | BLOCKER | No undefined terms |
| CON-031 | LOGIC | BLOCKER | No contradictions |
| CON-032 | LOGIC | MAJOR | No circular dependencies |
| CON-040 | CHANGE | BLOCKER | Large changes need impact analysis |
| CON-041 | CHANGE | MAJOR | Changes need rollback plan |
| CON-042 | CHANGE | MAJOR | Spec changes need approval |
| CON-050 | SECURITY | BLOCKER | No secrets in files |
| CON-051 | SECURITY | BLOCKER | Inputs must be sanitized |
| CON-060 | OUTPUT | BLOCKER | Fixed output paths |
| CON-061 | OUTPUT | BLOCKER | Fixed output formats |
| CON-062 | OUTPUT | MAJOR | No TODO-only stubs |

---

**END OF DOCUMENT**
