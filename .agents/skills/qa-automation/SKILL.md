---
name: qa-automation
description: >-
  Quality assurance, test automation, and anti-hallucination verification protocols for unit testing, integration tests, E2E scenarios, and adversarial input fuzzing.
---

# QA Automation Skill

Provides comprehensive test design frameworks, automated testing pipelines, and anti-hallucination verification protocols for QA Automation Engineers.

---

## 1. Core Workflows & Procedures

### Workflow 1: Acceptance Criteria & Ground-Truth Verification
1. Read Acceptance Criteria from `docs/PRD.md` and API contracts from `docs/ARCHITECTURE.md`.
2. Generate a Test Matrix mapping every acceptance criterion to automated test cases:
   - Happy Path Scenarios.
   - Negative / Validation Error Cases.
   - Boundary & Edge Cases.
   - Concurrency & Race Condition Scenarios.

### Workflow 2: Anti-Hallucination Gatekeeping (The Oracle Protocol)
- As the verification gatekeeper, QA must NEVER accept self-reported completion from developer agents.
- **Mandatory Verification Checks**:
  1. Execute unit/integration tests directly (`pytest`, `npm test`, `vitest`).
  2. Verify command exit code is strictly `0`.
  3. Inspect test logs to ensure tests were actually executed and not skipped or mock-faked.
  4. Run static type checking (`tsc --noEmit`, `mypy`) to confirm 0 type errors.

### Workflow 3: Automated Unit & Integration Testing
- Write isolated unit tests for backend business logic and frontend component render behaviors.
- Write API integration tests verifying end-to-end request/response flows, status codes, and DB persistence.
- Enforce meaningful test assertions on actual states, return values, and schema shapes (never assert just `response is not None`).

### Workflow 4: Adversarial Input Generation & Fuzzing
- Test APIs and UI inputs with adversarial edge cases:
  - Empty strings, whitespace-only strings, null/undefined payloads.
  - SQL injection payloads (`' OR 1=1 --`), XSS payloads (`<script>alert(1)</script>`).
  - Extreme values (maximum integer, negative numbers, ultra-long strings).
  - Malformed JSON structures and mismatched types.

---

## 2. Standard Output Specification (`docs/QA_REPORT.md`)

When QA completes verification of a feature, generate `docs/QA_REPORT.md`:

```markdown
# QA Verification & Anti-Hallucination Report: [Feature Name]

## 1. Ground Truth Test Execution Summary
- **Execution Command**: `npm test` / `pytest`
- **Exit Code**: `0` (Passing)
- **Total Test Cases**: [Total count]
- **Passed**: [Pass count]
- **Failed**: [Fail count]
- **Coverage**: [Code coverage %]

## 2. Acceptance Criteria Verification Matrix
| Story ID | Scenario | Status | Test Path | Exit Code |
| :--- | :--- | :--- | :--- | :--- |
| US-001 | Valid user registration | ✅ PASS | `tests/api/test_auth.py::test_register_success` | 0 |
| US-001 | Duplicate email registration | ✅ PASS | `tests/api/test_auth.py::test_duplicate_email` | 0 |

## 3. Discovered Defects & Hallucination Inconsistencies
- **[DEFECT-001]**: [Issue description, steps to reproduce, severity]
```
