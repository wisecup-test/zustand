# Adopt Core Library Pattern for State Management Middleware: Immutable State Updates

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase demonstrates a consistent pattern of using core state management libraries (Redux, Immer) and middleware composition across 24 files with 90.38% confidence
- State management middleware is distributed across multiple files including redux.ts, immer.ts, persist.ts, combine.ts, and ssrSafe.ts, indicating a modular middleware architecture
- Test files (vanilla/basic.test.ts, vanilla/shallow.test.tsx, shallow.test.tsx, vanilla/subscribe.test.tsx) show comprehensive testing of state management primitives
- Example applications (demo/src/App.jsx, starter/src/index.tsx) demonstrate practical usage patterns of the state management library
- Build configuration (rollup.config.mjs) and tooling (eslint.config.js) support library development and distribution

## Problem Statement

The project requires a standardized approach to state management that supports middleware composition, immutable updates, persistence, SSR safety, and Redux integration while maintaining testability and modularity across vanilla JavaScript and React implementations.

## Decision

1. MUST: Immutable state updates MUST be handled through the Immer middleware when complex nested state modifications are required

## Policy Block

- MUST Immutable state updates MUST be handled through the Immer middleware when complex nested state modifications are required

In scope:
- All state management middleware modules in src/middleware/
- Vanilla JavaScript state management implementations in tests/vanilla/
- React-specific state management hooks and components
- Example applications demonstrating library usage
- Build and distribution configuration for the library

Out of scope:
- Application-specific business logic outside of state management
- Third-party state management libraries not explicitly integrated
- Backend API implementations
- Non-state-related UI components

## Rationale

- The pattern appears consistently across 24 files with 90.38% confidence, indicating a deliberate architectural choice rather than ad-hoc implementation
- Modular middleware architecture enables flexible composition and allows developers to opt-in to specific features (Redux, Immer, persistence) as needed
- Comprehensive test coverage across vanilla and React implementations ensures reliability and prevents regressions
- Support for both vanilla JavaScript and React maximizes library applicability and adoption potential

## Consequences

Positive:
- Modular middleware design enables developers to compose only the features they need, reducing bundle size
- Comprehensive test coverage provides confidence in library stability and correctness
- Redux middleware integration enables seamless migration for existing Redux applications
- SSR-safe middleware prevents common hydration issues in server-rendered applications

Negative:
- Multiple middleware layers may introduce performance overhead for simple use cases
- Learning curve for developers unfamiliar with middleware composition patterns
- Maintenance burden increases with each additional middleware module
- Potential for middleware ordering issues if composition is not carefully managed

## Alternatives

- Use a monolithic state management library with all features built-in (rejected)
  Rejected because: Monolithic approach would increase bundle size for applications that only need basic state management, and reduces flexibility for customization
  When valid: Valid for applications that require all middleware features and prioritize simplicity over bundle size optimization
- Adopt a third-party state management solution like MobX or Recoil (rejected)
  Rejected because: Third-party solutions would not provide the same level of control over middleware composition and may not support the specific vanilla JavaScript + React dual-target requirement
  When valid: Valid for new projects without existing state management patterns or when third-party ecosystem is more important than customization
- Implement state management without middleware abstraction (rejected)
  Rejected because: Direct implementation would lead to code duplication across features like persistence, Redux integration, and SSR safety, reducing maintainability
  When valid: Valid for very simple applications with minimal state management requirements

## Risks

- Middleware composition order may cause unexpected behavior if not properly documented
  Mitigation: Provide clear documentation and examples showing correct middleware ordering, implement runtime warnings for common ordering mistakes
  Owner: engineering team
- Performance degradation in applications with frequent state updates due to middleware overhead
  Mitigation: Implement performance benchmarks in test suite, provide guidance on when to use lightweight alternatives, optimize hot paths in middleware implementations
  Owner: engineering team
- Breaking changes in middleware APIs could impact downstream consumers
  Mitigation: Follow semantic versioning strictly, maintain comprehensive changelog, provide migration guides for major version updates
  Owner: engineering team

## Implementation Notes

- Place all middleware implementations in src/middleware/ with consistent naming conventions (e.g., redux.ts, immer.ts, persist.ts)
- Ensure each middleware module exports a factory function that returns the middleware, enabling configuration at composition time
- Maintain separate test suites for vanilla (tests/vanilla/) and React-specific (tests/) implementations to ensure both targets work correctly
- Provide example applications in examples/ directory demonstrating common usage patterns and middleware combinations
- Configure build tooling (rollup.config.mjs) to produce separate bundles for each middleware module to enable tree-shaking

## Continuation Context


Verify commands:
- find src/middleware -name '*.ts' | wc -l | grep -E '[5-9]|[1-9][0-9]+'
- grep -r 'export.*middleware' src/middleware/ | wc -l | grep -E '[5-9]|[1-9][0-9]+'
- test -f src/middleware/redux.ts && test -f src/middleware/immer.ts && test -f src/middleware/persist.ts && test -f src/middleware/combine.ts && test -f src/middleware/ssrSafe.ts && echo 'Core middleware files present'
- npm test 2>&1 | grep -E '(PASS|✓).*test' | wc -l | grep -E '[1-9][0-9]+'

Accept when:
- At least 5 middleware modules exist in src/middleware/ directory with consistent TypeScript implementation
- Core middleware files (redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts) are present and export middleware factory functions
- Test suite passes with comprehensive coverage of vanilla and React implementations
- Example applications successfully demonstrate middleware composition patterns

## Enforcement

- Verified by: Automated CI pipeline runs test suite on every pull request
- Verified by: Code review process verifies new middleware follows established patterns
- Verified by: Build process validates that middleware modules are properly exported and tree-shakeable
- Violation handling: CI pipeline fails if tests do not pass or coverage drops below threshold
- Violation handling: Pull requests introducing non-composable middleware patterns are rejected during code review
- Violation handling: Build failures prevent deployment of improperly structured middleware modules
- Exception process: Exceptions for experimental middleware may be granted with explicit documentation of non-standard patterns
- Exception process: Performance-critical paths may bypass certain middleware layers with architectural review approval
- Exception process: Temporary exceptions documented in ADR amendments with sunset dates