---
name: anti-hallucination-guardian
description: >-
  Systematic protocols, verification gates, token optimization rules, and fact-checking workflows to eliminate AI hallucinations across code generation, API design, dependency resolution, and test execution.
---

# Anti-Hallucination Guardian Skill

Provides rigorous grounding protocols, token-optimized verification gates, and anti-fabrication procedures to ensure all agent outputs are tethered to ground truth while preventing token bloat.

---

## 1. The 4 Golden Rules of Anti-Hallucination & Verification

1. **The Grounding Rule (No Assumptions)**:
   - NEVER assume a file, class, method, column name, or API endpoint exists.
   - ALWAYS inspect the codebase using `find_by_name`, `grep_search`, or `view_file` before writing code that references it.

2. **The Verification Rule (Exit Codes > Self-Reports with Token Control)**:
   - NEVER trust an agent's self-proclaimed "All tests pass" or "Feature completed".
   - Ground truth verification must be deterministic (Exit code `0`).
   - **Token Optimization Protocol (Strict Zero-Test by Default)**:
     - By default, agents operate in **Implementation-Only Mode**: Do NOT write, execute, or read unit tests autonomously.
     - **Lightweight Verification Alternative**: Rely on fast static type checkers, syntax checkers, and linters (`tsc --noEmit`, `mypy --quick`, `ruff check`). Stop once static analysis passes (exit code 0).
     - **Testing Mode (Opt-In Only)**: Run or write unit tests ONLY when explicitly triggered by the user prompt (`"write test"`, `"unit test"`, `"test this"`, `"run tests"`, `"generate test suite"`). When active, enforce minimal test scope, log suppression flags, and a strict maximum of 2 auto-fix attempts.

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
Before marking any task or milestone as complete, run this verification checklist:

| Verification Target | Ground Truth Verification Method | Gate Criteria |
| :--- | :--- | :--- |
| **Files Created/Edited** | `view_file` or `find_by_name` | File physically exists and contains valid syntax. |
| **Code Syntax & Types (Default)** | Fast static check: `tsc --noEmit`, `mypy --quick`, `ruff check` | 0 errors returned (Exit code 0). |
| **Business Logic (Default Mode)** | Static type checking + Pydantic/Zod schema validation | Type check passes with exit code 0. No test files created/run. |
| **Business Logic (Testing Mode Only)** | Targeted single-file test with log suppression (`pytest -q --tb=short --maxfail=1`, etc.) | Test exit code = 0 (strictly max 2 auto-fix attempts). |
| **API Endpoints** | Integration test / curl verification (if explicitly requested) | Expected HTTP status code and response envelope. |

---

## 3. Strict Token Optimization & Test Control Guardrails

When working in this repository or any agent harness governed by this skill:

### A. Zero-Test Policy by Default (Implementation-Only Mode)
- **DO NOT WRITE TESTS**: Do not create, update, mock, or touch test files (`*.test.*`, `*.spec.*`, `tests/`, `__tests__/`).
- **DO NOT EXECUTE TESTS**: Do not run test runners (`pytest`, `vitest`, `jest`, `npm test`, `cargo test`, etc.) autonomously.
- **DO NOT READ TEST CONTEXT**: Do not load test files into context unless a broken import in production code strictly breaks compilation.
- **ONLY CODE & CONTRACT**: Focus 100% of reasoning and output on production code, signatures, and interfaces.

### B. Testing Mode Trigger & Execution Guardrails
- **Trigger**: Transition into Testing Mode ONLY if user prompt explicitly contains trigger words: `"write test"`, `"unit test"`, `"test this"`, `"run tests"`, or `"generate test suite"`.
- **Target Single Scope**: Test ONLY the immediate requested function/module.
- **Log Suppression**: Always pass flags to suppress tracebacks:
  - Python: `pytest <path> -q --tb=short --maxfail=1`
  - Node/TS: `npx vitest run <path> --reporter=compact` or `npm test -- <path> --bail`
  - Go: `go test -v -run <TestName> <pkg>`
  - Rust: `cargo test <test_name> -- --nocapture`

---

## 4. Self-Correction & Recovery Protocol (Max 2 Attempts)

If an agent discovers an error or a test fails (when in Testing Mode):
1. **Halt Execution Immediately**: Do not fabricate mock data or write workaround hacks.
2. **Read Concise Error**: Inspect only the concise error message or stack trace.
3. **Attempt 1 (Targeted Fix)**: Apply the direct, minimal fix to production code or test.
4. **Attempt 2 (Re-run Once)**: Re-run test once. If it still fails, **STOP IMMEDIATELY**.
5. **Revert & Escalate**: Revert broken test changes, report the exact error signature concisely (under 5 lines), and ask the user for direction. **NEVER loop beyond 2 attempts.**
