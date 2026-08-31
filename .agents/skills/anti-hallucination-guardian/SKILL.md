---
name: anti-hallucination-guardian
description: >-
  Systematic protocols, verification gates, and fact-checking workflows to eliminate AI hallucinations across code generation, API design, dependency resolution, and test execution.
---

# Anti-Hallucination Guardian Skill

Provides rigorous grounding protocols, verification gates, and anti-fabrication procedures to ensure all agent outputs are tethered to ground truth.

---

## 1. The 4 Golden Rules of Anti-Hallucination

1. **The Grounding Rule (No Assumptions)**:
   - NEVER assume a file, class, method, column name, or API endpoint exists.
   - ALWAYS inspect the codebase using `find_by_name`, `grep_search`, or `view_file` before writing code that references it.

2. **The Verification Rule (Exit Codes > Self-Reports)**:
   - NEVER trust an agent's self-proclaimed "All tests pass" or "Feature completed".
   - Success is ONLY validated by executing real unit tests (`pytest`, `npm test`, `vitest`) and receiving exit code `0`.

3. **The Package & API Grounding Rule (No Phantom Libraries)**:
   - NEVER import packages, NPM libraries, or PyPI dependencies without verifying they are defined in `package.json`, `requirements.txt`, `pyproject.toml`, or officially supported in standard libraries.
   - Do not invent hypothetical framework functions or deprecated parameters.

4. **The Evidence Citation Rule**:
   - Every claim of a bug fix, system status, or test result MUST be accompanied by exact file paths, line references, or actual command terminal output.

---

## 2. Hallucination Detection & Prevention Workflows

### Workflow 1: Pre-Execution Codebase Grounding
Before writing any code or architecture specification:
1. Search for existing models and types (`grep_search` for interface/type/class definitions).
2. Check existing routes and endpoints to avoid duplicate or conflicting paths.
3. Check dependency manifests (`package.json`, `poetry.lock`, `requirements.txt`) before importing third-party libraries.

### Workflow 2: Anti-Fabrication Code Review
When generating or reviewing code, actively scan for common hallucination patterns:
- **Phantom APIs**: Invoking methods that don't exist on third-party libraries (e.g. invented SDK methods).
- **Silent Exception Swallowing**: `try { ... } catch (e) {}` blocks that pretend operations succeed.
- **Mock Over-Fitting**: Writing tests that assert on hardcoded mock values instead of testing the actual business logic.
- **Fictional File References**: Linking to files or imports that were never created in the repository.

### Workflow 3: Ground-Truth Verification Gate (Pre-Handoff)
Before marking any task or milestone as complete, run this checklist:

| Verification Target | Ground Truth Verification Method | Gate Criteria |
| :--- | :--- | :--- |
| **Files Created/Edited** | `view_file` or `find_by_name` | File physically exists and contains valid syntax. |
| **Code Syntax & Types** | `tsc --noEmit`, `mypy`, or linter command | 0 errors returned. |
| **Business Logic** | `pytest` / `vitest` command | Test suite exit code = 0. |
| **API Endpoints** | Integration test / curl verification | Expected HTTP status code and response envelope. |

---

## 3. Self-Correction & Recovery Protocol

If an agent discovers it made an unverified assumption or a test fails:
1. **Halt Execution Immediately**: Do not fabricate mock data or write workaround hacks.
2. **Read the Ground Truth**: Inspect the exact error message, stack trace, or file content.
3. **Trace Root Cause**: Identify the discrepancy between the model's assumption and reality.
4. **Apply Corrective Fix**: Update the code to match the actual codebase reality and re-verify with tests.
