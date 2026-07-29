---
name: tdd-test
description: >-
  Write failing tests from product requirements without mocks. Use when the
  user asks for TDD, requirement-based tests, tdd-test, or to turn acceptance
  criteria into tests before implementing.
---

# TDD Test

Successful TDD keeps a strict Red-Green-Refactor cycle and designs code around failing tests. Writing tests first forces focus on requirements and interface design before implementation.

**DO NOT USE MOCKS.**

## Workflow

1. Parse each product requirement into discrete, testable assertions.
2. Detect the project's test stack (framework, layout, helpers, existing auth/route tests). Reuse it; do not invent a new framework.
3. **Red**: Write a minimal, focused, real (no-mock) failing test for one target behavior. Run it and confirm it fails for the right reason (missing behavior, not setup errors).
4. **Stop and ask** before Green or Refactor. Summarize what was asserted and ask whether to implement.
5. When the user approves:
   - **Green**: Write only the simplest production code required to make the failing test pass.
   - **Refactor**: Clean up production and test code while the suite stays green.

## Requirement → assertions

Translate product language into observable outcomes: status codes, redirects, denied access, visible UI, persisted data.

**Example:** "user must be authenticated and authorized to use healthcare dashboard"

- Unauthenticated request is denied or redirected
- Authenticated but unauthorized role is denied
- Authenticated and authorized user can access

Write separate tests for each scenario (positive and negative).

## DO NOT USE MOCKS

Non-negotiable. Overrides any conflicting generic TDD advice.

- No `jest.mock`, `vi.mock`, `unittest.mock`, sinon fakes, or stubs for domain collaborators
- Prefer real HTTP against the app, real auth session/token setup, real DB / testcontainers / project fixtures
- Allowed: test data factories, real test users, DB seeders, local/in-process DB already used by the project
- Never stub auth or authorization away

## Red-Green-Refactor

- **Red**: Write a minimal, focused test that describes a single target behavior and observe it fail.
- **Green**: Write only the simplest production code required to make that failing test pass.
- **Refactor**: Clean up newly added production code and test code while the suite remains green.

## High-quality tests

- **Test behavior, not implementation**: Assert against the public API and expected outputs, not internal mechanics.
- **Positive and negative**: Ideal path plus invalid data, errors, and unauthorized access.
- **One scenario per test**: Failures must be immediately diagnostic.
- **Fully isolated**: Independent, reproducible; no reliance on other tests' order or state.

## Execution speed

- Keep the feedback loop tight: prefer the fastest *real* harness the project already supports.
- Do **not** use mocks, stubs, or test doubles to fake collaborators.
- Speed via real local fixtures: in-memory or ephemeral DB, seeders, factories — not by mocking away the system under test.
- Run the relevant tests constantly during the cycle; rely on CI for the full suite.

## Test code is first-class

- Same quality, style, and architecture standards as production code.
- Name tests as behavioral specifications (feature + expected outcome).
- Refactor tests to remove duplication, streamline fixtures, and keep them scannable.

## Coverage discipline

- Do not chase 100% coverage blindly.
- Prioritize complex logic, edge cases, and high-risk business rules over boilerplate getters/setters.
