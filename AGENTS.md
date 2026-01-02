# Repository Guidelines

## Project Structure & Source of Truth
Spec-first repo. Canonical order: `00_high_level_plan.md` → `01_constraints.md` → `11_interfaces.ir.yml` → `10_architecture.ir.yml` → `20_impl_plan.ir.yml` → `30_code_status.ir.yml`; smoke expectations in `21_test_plan.ir.yml`. Archive old plans in `spec/history/` before edits. `GEMINI.md` and `.claude` are agent notes; create `src/`, `tests/`, and `scripts/` only when implementation matches `20_impl_plan.ir.yml`.

## Editing the IR
- Module IDs and interfaces must come from `11_interfaces.ir.yml`; never invent new names in code.
- Clear `TODO` markers before generating artifacts (`rg "TODO" spec` is the fastest scan).
- Keep YAML/Markdown tidy (2-space indent, ~100-char wrap) and describe rationale when changing flows.

## Build, Test, and Development Commands
- Currently spec-only; run your preferred Markdown/YAML lint if available.
- When code appears: C++ via `cmake -S . -B build && cmake --build build`, Python via `pytest -q`, TypeScript via `npm test`. Smoke paths should mirror `ctest -R <id>` targets tied to `21_test_plan.ir.yml`.
- Use `rg "<term>" spec` before edits to keep references consistent.

## Coding Style & Naming Conventions
- YAML/Markdown: concise headings, 2-space indent.
- C++17: `clang-format`, `snake_case` functions/variables, `PascalCase` types; headers self-contained.
- Python 3.10+: `black` + `isort`, `snake_case` modules/functions, `PascalCase` classes.
- TypeScript: `prettier` + `eslint`, `kebab-case` packages, `camelCase` functions, `PascalCase` components/types. Align paths with module IDs.

## Testing Guidelines
- Implement smoke tests from `21_test_plan.ir.yml` first and update their status there.
- Name tests `*_test.cpp`, `test_*.py`, `<name>.spec.ts`; keep fixtures/mocks beside modules and deterministic.
- Record new test commands in the plan so CI and local runs stay aligned.

## Commit & Pull Request Guidelines
- No Git history yet; when initialized, favor short imperative Conventional Commits (e.g., `docs: tighten constraints`, `plan: add connectivity layout`).
- PRs should summarize intent, list touched spec files, note interface/module ID changes, and include test/validation output. Land IR updates before dependent code.

## Security & Configuration Tips
- Never commit secrets or proprietary paths; provide sample configs and use env vars.
- Document platform or hardware assumptions in `01_constraints.md` when adding dependencies.

## Agent Collaboration Notes
- Follow roles in `01_constraints.md` §5: Gemini (architecture), Codex (implementation layout), Claude (deep code). Respect boundaries unless the IR is updated.
- IR-first workflow: propose text, update specs, then code. If an identifier is missing from `11_interfaces.ir.yml`, pause and extend it before implementation.
