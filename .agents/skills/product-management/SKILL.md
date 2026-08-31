---
name: product-management
description: >-
  Expert product management workflows for requirement analysis, drafting PRDs (Product Requirement Documents), breaking down features into structured User Stories with Acceptance Criteria, and backlog prioritization.
---

# Product Management Skill

Provides structured workflows, rubrics, and templates for technical Product Managers to transform user needs and business goals into actionable engineering specifications.

---

## 1. Core Workflows & Procedures

### Workflow 1: Requirement Analysis & Discovery
1. Extract explicit requirements, implicit constraints, user personas, and target outcomes from the user prompt.
2. Identify dependencies, technical risks, ambiguities, and out-of-scope boundaries.
3. Validate technical feasibility with Software Architect before finalizing scope.

### Workflow 2: PRD (Product Requirement Document) Generation
Structure the PRD systematically:
- **Executive Summary & Goal**: The problem statement and targeted business/user impact.
- **User Personas & Journeys**: Who interacts with this feature and key workflow steps.
- **Functional Requirements**: Core capabilities categorized by priority (MoSCoW framework: Must, Should, Could, Won't).
- **Non-Functional Requirements (NFRs)**: Latency, throughput, security, compliance, accessibility.
- **Success Metrics / KPIs**: Measurable indicators of feature adoption and stability.

### Workflow 3: User Story & Acceptance Criteria Formulation
Format all stories following the standard Gherkin / BDD convention:
- **Story Format**: `As a <User Persona>, I want to <Action/Goal>, so that <Benefit/Value>.`
- **Acceptance Criteria (Given-When-Then)**:
  ```gherkin
  Scenario: Successful submission
    Given the user is authenticated and has valid permissions
    When the user submits the form with valid payload
    Then the system creates the record and returns HTTP 201 Created
  ```

### Workflow 4: Backlog Prioritization & Milestone Mapping
- Organize stories into milestones (e.g., Milestone 1: Core API & DB, Milestone 2: UI & Integration).
- Enforce Definition of Ready (DoR) and Definition of Done (DoD) before handoff to engineering.

---

## 2. Standard Output Specification (`docs/PRD.md`)

When executing PM tasks, output the formal document to `docs/PRD.md` using the following schema:

```markdown
# Product Requirement Document (PRD): [Feature Name]

## 1. Overview & Objective
- **Problem Statement**: ...
- **Target Outcome**: ...

## 2. Personas & Scope
- **Target Persona**: ...
- **In-Scope**: ...
- **Out-of-Scope**: ...

## 3. Epics & User Stories
### Epic 1: [Epic Name]
#### US-001: [Story Title]
- **Story**: As a [user], I want to [action] so that [benefit].
- **Acceptance Criteria**:
  - [ ] Given [precondition], When [trigger], Then [expected result].

## 4. Non-Functional Requirements
- Security, Performance, Availability, Compliance.
```
