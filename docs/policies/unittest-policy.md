# AGENT HARNESS EXECUTION DIRECTIVE: STRICT TOKEN OPTIMIZATION & TEST CONTROL

## 1. DEFAULT BEHAVIOR: STRICT ZERO-TEST POLICY (OPT-IN ONLY)
By default, you are in **Implementation-Only Mode**. You MUST strictly follow these rules:
- **DO NOT WRITE TESTS:** Do not create, update, mock, or touch any test files (`*.test.*`, `*.spec.*`, `tests/`, `__tests__/`).
- **DO NOT EXECUTE TESTS:** Do not execute test runners (`pytest`, `vitest`, `jest`, `npm test`, `cargo test`, etc.) autonomously under any circumstances.
- **DO NOT READ TEST CONTEXT:** Do not load or read existing test files into context unless a broken import in production code strictly breaks compilation.
- **ONLY CODE & CONTRACT:** Focus 100% of reasoning and output on production code, signatures, and interfaces.

---

## 2. EXCEPTION TRIGGER (EXPLICIT OPT-IN)
Only transition into **Testing Mode** if the user prompt explicitly uses trigger words such as:
`"write test"`, `"unit test"`, `"test this"`, `"run tests"`, or `"generate test suite"`.

---

## 3. GUARDRAILS WHEN TESTING MODE IS ACTIVATED
When explicitly instructed to test, enforce these token-saving guardrails:

### A. Minimal Context & Scope
- Test ONLY the immediate function or module requested. Do not attempt full-suite test coverage.
- Generate tests based on the function interface/signatures. Do not read unrelated codebase files for mocks; infer or use standard minimal stubs.

### B. Execution & Log Suppression
- If running tests, target ONLY the single test file or function. Never run the full test suite.
- Always use minimal output flags to suppress traceback tokens:
  - **Python:** `pytest <path> -q --tb=short --maxfail=1`
  - **Node/TS:** `npx vitest run <path> --reporter=compact` or `npm test -- <path> --bail`
  - **Go:** `go test -v -run <TestName> <pkg>`
  - **Rust:** `cargo test <test_name> -- --nocapture` (stop on first failure)

### C. Strict Auto-Fix Loop Limit (Max 2 Attempts)
- You are strictly limited to a **maximum of 2 auto-fix attempts** if a test fails.
- **Iteration 1:** Read concise failure -> apply targeted fix.
- **Iteration 2:** Re-run once. If it still fails, STOP IMMEDIATELY.
- Revert broken test changes, report the exact error signature concisely (under 5 lines), and ask the user for direction. NEVER loop beyond 2 attempts.

---

## 4. LIGHTWEIGHT VERIFICATION ALTERNATIVE
For baseline correctness without token bloat, prefer static type checkers and linters over unit tests:
- Run fast syntax/type checks only (e.g., `tsc --noEmit`, `mypy --quick`, `ruff check`).
- Stop once static analysis passes.
