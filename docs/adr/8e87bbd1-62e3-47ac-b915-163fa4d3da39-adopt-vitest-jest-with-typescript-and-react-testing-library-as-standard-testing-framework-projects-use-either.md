# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Projects Use Either

Status: proposed
Date: 2024-01-15
Deciders: Detection Pipeline (automated)

## Context

- The codebase contains multiple test files using TypeScript and TSX extensions, indicating a React-based application requiring component testing capabilities
- Test files are organized in a dedicated tests directory with subdirectories (vanilla, etc.) suggesting separation of concerns between different testing contexts
- The presence of specialized test files (immer.test.tsx, ssr.test.tsx, shallow.test.tsx) indicates testing of advanced React patterns including state management, server-side rendering, and shallow comparison optimizations
- The test.tsx and test.ts extensions combined with the file naming pattern suggest adoption of a modern JavaScript testing framework compatible with TypeScript and JSX
- Six test files with high significance scores (0.92-0.93) demonstrate consistent application of the testing pattern across the codebase

## Problem Statement

The project requires a standardized testing approach that supports TypeScript, React components, advanced state management patterns (Immer), server-side rendering scenarios, and both vanilla JavaScript and React-specific test cases. Without a consistent testing framework and strategy, test quality, maintainability, and developer productivity would suffer.

## Decision

1. MAY: Projects MAY use either Vitest or Jest as the underlying test runner, provided they meet the TypeScript and React requirements

## Policy Block

- MAY Projects MAY use either Vitest or Jest as the underlying test runner, provided they meet the TypeScript and React requirements

In scope:
- All unit tests for React components
- All unit tests for vanilla JavaScript/TypeScript modules
- Integration tests involving state management libraries
- Server-side rendering (SSR) test scenarios
- Shallow comparison and optimization tests

Out of scope:
- End-to-end (E2E) tests using browser automation tools
- Performance benchmarking tests
- Visual regression tests
- Load and stress testing

## Rationale

- The pattern evidence shows 6 test files with 92.37% confidence, indicating a well-established and consistent testing approach across the codebase
- TypeScript and TSX extensions provide type safety and catch errors at compile time, reducing runtime bugs and improving developer experience
- Modern testing frameworks like Vitest and Jest with React Testing Library support are industry standards that provide excellent TypeScript integration, fast execution, and comprehensive React component testing capabilities
- Organizing tests by context (vanilla vs React-specific) improves maintainability and allows developers to quickly locate and understand test coverage

## Consequences

Positive:
- Type-safe tests catch errors early and provide better IDE support with autocomplete and inline documentation
- Consistent testing patterns across the codebase reduce cognitive load and onboarding time for new developers
- Support for advanced React patterns (Immer, SSR, shallow comparison) ensures comprehensive test coverage for complex scenarios
- Organized test directory structure makes it easy to locate, maintain, and extend test suites

Negative:
- TypeScript compilation adds slight overhead to test execution time compared to plain JavaScript
- Developers must learn TypeScript and React Testing Library APIs if not already familiar
- Maintaining type definitions for test utilities and mocks requires additional effort
- Complex React patterns may require more sophisticated test setup and teardown logic

## Alternatives

- Use plain JavaScript (.test.js) for all tests without TypeScript (rejected)
  Rejected because: Loses type safety benefits, increases runtime errors, and is inconsistent with the TypeScript-based application codebase
  When valid: Only valid for pure JavaScript projects without TypeScript
- Use Enzyme instead of React Testing Library for component testing (rejected)
  Rejected because: Enzyme is deprecated and no longer actively maintained; React Testing Library is the recommended approach by the React team
  When valid: Only for legacy codebases already heavily invested in Enzyme
- Colocate test files next to source files instead of a dedicated tests directory (deferred)
  Rejected because: Not rejected, but current pattern uses dedicated tests directory
  When valid: Valid for projects preferring colocation; both approaches have merit depending on project size and team preference

## Risks

- Test execution time may increase as the test suite grows, especially with TypeScript compilation overhead
  Mitigation: Use watch mode during development, implement parallel test execution, and consider test sharding for CI/CD pipelines
  Owner: Engineering team
- Complex React patterns (SSR, Immer integration) may be difficult to test correctly, leading to flaky or incomplete tests
  Mitigation: Establish test patterns and examples for common scenarios, provide team training, and maintain a test utilities library for common setup
  Owner: Engineering team
- Framework lock-in may make it difficult to migrate to alternative testing solutions in the future
  Mitigation: Abstract framework-specific APIs behind custom test utilities where possible, and follow testing best practices that are framework-agnostic
  Owner: Engineering team

## Implementation Notes

- Configure the test runner (Vitest or Jest) with TypeScript support using ts-jest or native TypeScript handling
- Set up React Testing Library with appropriate render utilities and custom queries for the application's component patterns
- Create test utility files for common setup patterns (e.g., store configuration for Immer tests, SSR environment setup)
- Establish naming conventions: use descriptive test file names that mirror the source file being tested (e.g., ComponentName.test.tsx for ComponentName.tsx)
- Configure IDE and editor plugins for test runner integration to enable inline test execution and debugging

## Continuation Context


Verify commands:
- find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l
- grep -r "import.*@testing-library/react" tests/ || grep -r "from '@testing-library/react'" tests/
- test -f vitest.config.ts || test -f jest.config.ts || test -f jest.config.js

Accept when:
- All test files in the tests directory use .test.ts or .test.tsx extensions
- Test configuration file (vitest.config.ts or jest.config.ts) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (.test.tsx)

## Enforcement

- Verified by: CI/CD pipeline runs all tests and fails on test failures or missing test coverage
- Verified by: Code review process checks for test file presence and proper naming conventions
- Verified by: Linting rules enforce test file naming patterns and TypeScript usage
- Violation handling: Pull requests without tests for new features or bug fixes are rejected
- Violation handling: Test files not following naming conventions (.test.ts/.test.tsx) trigger CI warnings
- Violation handling: Code coverage reports are generated and reviewed; significant coverage drops block merges
- Exception process: Exceptions for test coverage may be granted for experimental features or prototypes with explicit documentation
- Exception process: Alternative test file locations (outside tests directory) require architectural review and approval
- Exception process: Temporary test skipping (using .skip) requires a linked issue and must be resolved within one sprint