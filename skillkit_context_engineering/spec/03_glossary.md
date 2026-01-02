# 03. Glossary

> **Purpose**: This document defines all terms, abbreviations, and symbols used in this project. If a term is used without being defined here, it violates constraint CON-030.

---

## 1. How to Use

1. **Before using a term**: Check if it's defined here
2. **If not defined**: Add it before using
3. **If ambiguous**: Clarify the definition

## 2. Format

Each entry follows this structure:
```
**Term** (abbreviation, if any)
: Definition. Context or usage notes if needed.
```

---

## 3. Core Terms

**Agent**
: An AI model instance (Gemini, Codex, Claude) that performs a specific role in the workflow.

**BLOCKER**
: A severity level indicating that failure stops the entire pipeline. Cannot proceed until resolved.

**Casefile**
: The `00_casefile.md` document that defines the objective, constraints, and context for a single run.

**Checklist**
: A collection of atomic, testable quality items defined in `checklists/checklist.yaml`.

**Constraint**
: A MUST or MUST NOT rule defined in `spec/01_constraints.md` with a unique ID (CON-XXX).

**Evidence**
: Artifacts (logs, screenshots, command outputs) that prove a claim or demonstrate compliance.

**Gate**
: A collection of checklist items executed together, defined in `gates/*.yaml`.

**Handoff**
: The transfer of work from one agent to another, using fixed output paths and formats.

**MAJOR**
: A severity level indicating that failure should be documented and planned for fix. Can proceed with caution.

**MINOR**
: A severity level indicating that failure should be noted but does not block progress.

**Operator**
: The human who coordinates the 3-CLI workflow by copy-pasting prompts and managing runs.

**Pack**
: A subset of checklist items grouped for a specific purpose (e.g., `pack-core.yaml`, `pack-docs.yaml`).

**Report**
: The output of gate execution, stored in `reports/latest/` and archived to `reports/archive/`.

**Rubric**
: A scoring guide for semantic evaluation, defined in `eval/rubrics/*.md`.

**Run**
: A single execution cycle, stored in `ops/RUNS/<run-name>/`.

**Skill**
: A reusable procedure document defining how to perform a specific type of work, stored in `skills/`.

**Spec**
: Short for specification. The immutable rules defined in `spec/*.md`.

---

## 4. Technical Terms

**API** (Application Programming Interface)
: A defined interface for software components to communicate.

**CI** (Continuous Integration)
: Automated build and test processes triggered by code changes.

**CLI** (Command Line Interface)
: A text-based interface for interacting with software.

**Diff**
: The difference between two versions of a file or codebase.

**JSON** (JavaScript Object Notation)
: A lightweight data interchange format.

**L0, L1, L2** (Gate Levels)
: Shorthand for gate types. L0=Preflight, L1=CI, L2=Semantic.

**YAML** (YAML Ain't Markup Language)
: A human-readable data serialization format used for configuration.

---

## 5. Project-Specific Terms

*Add project-specific terms here as needed.*

**[EXAMPLE] ONN** (Ontology Neural Network)
: A neural network architecture that incorporates ontological knowledge. (If used in your project)

**[EXAMPLE] CSA** (Cognitive Safety Architecture)
: An architecture pattern for safe AI systems. (If used in your project)

---

## 6. Abbreviations Quick Reference

| Abbr | Full Form | Context |
|------|-----------|---------|
| API | Application Programming Interface | Software interfaces |
| CI | Continuous Integration | Build/test automation |
| CLI | Command Line Interface | Terminal interfaces |
| CON | Constraint | Spec constraint IDs |
| CHK | Checklist | Checklist item IDs |
| ENV | Environment | System configuration |
| ID | Identifier | Unique identifiers |
| L0/L1/L2 | Gate Levels | Preflight/CI/Semantic |
| YAML | YAML Ain't Markup Language | Config files |

---

## 7. Adding New Terms

When adding a new term:

1. Place it in the appropriate section (Core, Technical, Project-Specific)
2. Keep definitions concise (1-2 sentences)
3. Include context if the term is ambiguous
4. Update if the definition evolves

---

**END OF DOCUMENT**
