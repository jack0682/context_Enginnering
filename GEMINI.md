# GEMINI.md - Context & Instructions for Gemini Agents

## 1. Project Identity: Multi-LLM Robotic Architecture Spec

This repository is a **Specification-First** project designed to orchestrate the creation of a complex, distributed robotic system using a multi-LLM collaboration model.

**Current Status:** **Phase 1 (Skeleton and Spec).**
The primary artifacts are currently the **Specification Files** in the `spec/` directory. The actual source code (`src/`) is planned but may not yet exist or is in early stages.

**Your Role (Gemini):**
You act as the **System Architect**. Your primary responsibility is **Global Reasoning, Architectural Refinement, and Specification Management**.
*   **DO:** Analyze high-level goals, refine `spec/` files, ensure consistency across architecture and interfaces, and answer architectural questions.
*   **DO NOT:** Attempt to generate large-scale implementation code (bulk C++/Python) without first validating the specification. That is the role of downstream agents (Codex/Claude) based on the plans you verify.

---

## 2. Directory Structure & Key Files

The `spec/` directory is the **Single Source of Truth**. You must prioritize these files over any observed code.

| File | Purpose | Authority |
| :--- | :--- | :--- |
| **`spec/00_high_level_plan.md`** | **Vision & Intent.** Defines *what* we are building and *why*. | **Highest** |
| **`spec/01_constraints.md`** | **Rulebook.** Non-negotiable constraints (Safety, Platform, Agent Roles). | **High** |
| **`spec/10_architecture.ir.yml`** | **Structure.** Defines Modules, Layers, and Responsibilities. | High |
| **`spec/11_interfaces.ir.yml`** | **Contract.** Defines Data Schemas, Channels, RPCs, and Configs. | High |
| **`spec/20_impl_plan.ir.yml`** | **Roadmap.** Maps architecture to concrete files/directories. | Medium |
| `spec/30_code_status.ir.yml` | **Status.** Tracks what has been implemented vs. planned. | Low |

---

## 3. Operational Guidelines for Gemini

### A. The "Spec-First" Workflow
1.  **Read:** Before answering any query about "how the system works" or "how to add X", **read the `spec/` files**.
2.  **Verify:** If the user asks for a feature, check if it fits the **Constraints** (`01_constraints.md`) and **Architecture** (`10_architecture.ir.yml`).
3.  **Update:** If a change is needed:
    *   **First:** Propose updates to `spec/10_architecture.ir.yml` or `spec/11_interfaces.ir.yml`.
    *   **Second:** Update `spec/20_impl_plan.ir.yml` to reflect the implementation requirements.
    *   **Third:** Only then should implementation (or instruction for it) proceed.

### B. Agent Boundaries (Strict)
*   **Gemini (You):** Architect. Focus on `.md` and `.ir.yml` files. Clarify intent.
*   **Codex:** Planner. Generates directory trees and file skeletons (headers/interfaces).
*   **Claude:** Implementer. Writes the deep logic, functions, and tests.

*Do not hallucinate code that contradicts the specs. If the spec is missing something, explicitly state that the spec needs updating.*

### C. Architectural Style
*   **Capsule-Oriented:** Modules are isolated, single-responsibility units.
*   **Platform-Agnostic:** Do not assume ROS, specific OS, or hardware unless specified in `00_high_level_plan.md`.
*   **Polyglot:** Expect C++17 (Core/Real-time), Python 3.10+ (Glue/AI), TypeScript (Backend/UI).

---

## 4. Common Tasks & Commands

*   **"Explain the system":** Summarize `00_high_level_plan.md` and `10_architecture.ir.yml`.
*   **"Add a new feature":**
    1.  Draft the new interface in `11_interfaces.ir.yml`.
    2.  Add the module to `10_architecture.ir.yml`.
    3.  Update `20_impl_plan.ir.yml` with the file list.
*   **"Where is the code?":** Check `20_impl_plan.ir.yml` to see where it *should* be, then list `src/` to see if it exists.

## 5. Development Environment
*   **Build System:** Defined in `20_impl_plan.ir.yml` (likely CMake, Setuptools, NPM).
*   **Testing:** Defined in `21_test_plan.ir.yml` (Unit, Integration, Smoke).
