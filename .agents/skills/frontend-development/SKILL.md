---
name: frontend-development
description: >-
  Frontend development workflows for building responsive UI components, client-side state management, Mock API servers, and UX specification compliance.
---

# Frontend Development Skill

Provides UI/UX component development guidelines, state management patterns, and token-optimized implementation workflows for Senior Frontend Engineers.

---

## ⚡ Token Optimization Note (Implementation-Only Mode)
By default, frontend engineers operate in **Implementation-Only Mode**:
- Do not write, execute, or read component unit test files unless explicitly requested by the user (`"write test"`, `"test this"`, etc.).
- Rely on TypeScript static type checking (`tsc --noEmit`) and component contract validation for deterministic baseline verification.

---

## 1. Core Workflows & Procedures

### Workflow 1: Component Architecture & Implementation
1. Review UX requirements and user stories in `docs/PRD.md`.
2. Break screens down into modular, atomic components (Atoms -> Molecules -> Organisms -> Pages).
3. Implement responsive layouts adhering to modern CSS frameworks (TailwindCSS, CSS Modules, or styled components).
4. Ensure accessibility compliance (ARIA labels, keyboard navigation, semantic HTML tags).

### Workflow 2: State Management & Data Fetching
- Separate server cache state (e.g. React Query / SWR / TanStack Query) from local UI state (useState / Pinia / Redux).
- Handle all 4 visual states across all data fetching components:
  1. **Loading State**: Skeletons or spinners.
  2. **Empty State**: Friendly illustration and call to action.
  3. **Error State**: Actionable error message with a retry button.
  4. **Success / Content State**: Rendered UI with data.

### Workflow 3: Mock API & Local Development Environment
- Create deterministic mock handlers (MSW - Mock Service Worker, Mirage, or mock JSON servers) based on `docs/ARCHITECTURE.md`.
- Enable full local frontend testing independent of backend deployment availability.

### Workflow 4: Form Handling & Client-Side Validation
- Implement schema-based validation (Zod, Yup, Joi) mirroring backend rules.
- Display inline field errors and disable submit actions during pending mutations to prevent double submissions.

---

## 2. Frontend Verification Checklist

Before completing frontend tasks, verify:
- [ ] Responsive design functions across mobile, tablet, and desktop viewports.
- [ ] Loading, Empty, and Error states are implemented.
- [ ] Static type checks pass with 0 errors (`tsc --noEmit`).
- [ ] Strict Zero-Test Policy observed: component unit tests run ONLY if explicitly requested by user prompt.
- [ ] No layout shift (CLS) or console warning errors during render.
