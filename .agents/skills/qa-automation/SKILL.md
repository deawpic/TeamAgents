---
name: qa-automation
description: >-
  Quality assurance, test automation, token optimization guardrails, and anti-hallucination verification protocols for unit testing, integration tests, and static verification.
---

# QA Automation Skill

Provides token-optimized quality assurance standards, static verification gates, and anti-hallucination protocols for QA Automation Engineers and Verification Gatekeepers.

---

## ⚡ Strict Token Optimization & Test Control Directive (Core Invariant)

This skill strictly enforces the **Zero-Test Policy by Default** to prevent context window bloat and excessive token consumption across agent interactions.

### 1. Default Behavior: Strict Zero-Test Policy (Implementation-Only Mode)
By default, the QA Agent and all peer agents operate in **Implementation-Only Mode**:
- **DO NOT WRITE TESTS**: Do not create, update, mock, or touch any test files (`*.test.*`, `*.spec.*`, `tests/`, `__tests__/`).
- **DO NOT EXECUTE TESTS**: Do not execute test runners (`pytest`, `vitest`, `jest`, `npm test`, `cargo test`, etc.) autonomously under any circumstances.
- **DO NOT READ TEST CONTEXT**: Do not load or read existing test files into context unless a broken import in production code strictly breaks compilation.
- **ONLY CODE & CONTRACT**: Focus 100% of reasoning and verification output on production code, schema models, and interfaces.

### 2. Exception Trigger (Explicit Opt-In Only)
Transition into **Testing Mode** ONLY if the user prompt explicitly uses trigger words:
`"write test"`, `"unit test"`, `"test this"`, `"run tests"`, or `"generate test suite"`.

### 3. Guardrails When Testing Mode is Activated
When explicitly instructed to test, enforce these token-saving guardrails:
- **Minimal Context & Scope**:
  - Test ONLY the immediate function or module requested. Do not attempt full-suite test coverage.
  - Generate tests based on the function interface/signatures. Do not read unrelated codebase files for mocks; infer or use standard minimal stubs.
- **Execution & Log Suppression**:
  - Target ONLY the single test file or function. Never run the full test suite.
  - Always use minimal output flags to suppress traceback tokens:
    - **Python**: `pytest <path> -q --tb=short --maxfail=1`
    - **Node/TS**: `npx vitest run <path> --reporter=compact` or `npm test -- <path> --bail`
    - **Go**: `go test -v -run <TestName> <pkg>`
    - **Rust**: `cargo test <test_name> -- --nocapture` (stop on first failure)
- **Strict Auto-Fix Loop Limit (Max 2 Attempts)**:
  - You are strictly limited to a **maximum of 2 auto-fix attempts** if a test fails:
    - **Iteration 1**: Read concise failure -> apply targeted fix.
    - **Iteration 2**: Re-run once. If it still fails, **STOP IMMEDIATELY**.
    - Revert broken test changes, report the exact error signature concisely (under 5 lines), and ask the user for direction. **NEVER loop beyond 2 attempts.**

### 4. Lightweight Verification Alternative (Default Baseline Truth)
For baseline correctness without token bloat, prefer static type checkers and linters over unit tests:
- Run fast syntax/type checks only (e.g., `tsc --noEmit`, `mypy --quick`, `ruff check`).
- Stop once static analysis passes with exit code `0`.

---

## 1. Core Workflows & Procedures

### Workflow 1: Contract & Schema Verification (Default Mode)
1. Read Acceptance Criteria from `docs/PRD.md` and API contracts from `docs/ARCHITECTURE.md`.
2. Verify that production models and route handlers match the specified types and constraints.
3. Validate that request payloads are strictly validated using schema libraries (Pydantic / Zod).

### Workflow 2: Anti-Hallucination Gatekeeping (The Oracle Protocol)
- As the verification gatekeeper, QA must NEVER accept self-reported completion from developer agents without deterministic proof:
  1. **Default Mode (Lightweight Verification)**:
     - Run static type checking (`tsc --noEmit`, `mypy --quick`).
     - Run linter check (`ruff check`, `eslint`).
     - Verify command exit code is strictly `0` with 0 errors.
  2. **Testing Mode (Explicit User Opt-In Only)**:
     - Run targeted single test command with log suppression flags.
     - Verify test execution exit code is strictly `0`.
     - Strictly enforce the 2-attempt limit if failures occur.

### Workflow 3: Adversarial Input Analysis (Static & Schema-Level)
- Audit schemas and business rules against edge cases without generating redundant test files:
  - Null/undefined handling, empty strings, boundary limits.
  - Malformed payload rejection via schema validators.

---

## 2. Standard Output Specification (`docs/QA_REPORT.md`)

When QA completes verification of a feature, generate `docs/QA_REPORT.md`:

```markdown
# QA Verification & Anti-Hallucination Report: [Feature Name]

## 1. Verification Mode & Summary
- **Mode**: `Lightweight Static Check (Token-Optimized)` OR `Explicit Unit Test Execution`
- **Command Executed**: `tsc --noEmit` / `mypy --quick` / `pytest <path> -q --tb=short --maxfail=1`
- **Exit Code**: `0` (Passing)
- **Errors/Warnings**: 0

## 2. Acceptance Criteria Verification Matrix
| Story ID | Specification / Scenario | Verification Method | Status | Exit Code |
| :--- | :--- | :--- | :--- | :--- |
| US-001 | Valid user registration schema | Static Type & Schema Check | ✅ PASS | 0 |
| US-001 | Duplicate email constraint | Schema / Type Contract | ✅ PASS | 0 |

## 3. Discovered Defects & Inconsistencies
- (None / Defect description if found)
```
