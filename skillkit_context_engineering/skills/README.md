# Skills Directory

> **Purpose**: This directory contains reusable skill procedures organized by domain.

## Directory Structure

```
skills/
├── authoring/       # Writing and documentation skills
├── coding/          # Programming and development skills
├── research/        # Research and analysis skills
└── context/         # Context management skills
```

## How to Use Skills

1. **Identify the task type**: What kind of work are you doing?
2. **Select relevant skills**: Find procedures in the appropriate subdirectory
3. **Inject into prompt**: Include skill content when prompting agents
4. **Follow procedures**: Execute steps as documented

## Skill Selection Guide

| Task Type | Recommended Skills |
|-----------|-------------------|
| Writing docs | `authoring/evidence-first.md`, `authoring/notation-consistency.md` |
| Writing code | `coding/contract-safety.md`, `coding/test-first-changes.md` |
| Debugging | `coding/debugging-protocol.md` |
| Research | `research/structured-extraction.md`, `research/citation-hygiene.md` |
| Large context | `context/compression.md`, `context/conflict-resolution.md` |
| Papers | All authoring + `research/citation-hygiene.md` |

## Adding New Skills

1. Create a new `.md` file in the appropriate subdirectory
2. Follow the skill template:
   ```markdown
   # Skill: [Name]
   
   ## Purpose
   ## When to Use
   ## Procedure
   ## Evidence
   ## Common Mistakes
   ```
3. Update this README
