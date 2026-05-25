# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Test Files Use

These rules are ALWAYS ACTIVE for all test files in the project's tests directory and any unit tests for React components, vanilla JavaScript/TypeScript modules, integration tests involving state management libraries, and server-side rendering test scenarios.

### Rules

- **R-TEST-001** MUST: All test files MUST use TypeScript with .test.ts or .test.tsx extensions.
- **R-TEST-002** MUST: Test configuration file (vitest.config.ts or jest.config.ts) MUST exist and include TypeScript support.
- **R-TEST-003** MUST: React Testing Library MUST be imported and used in component test files (.test.tsx).
- **R-TEST-004** SHOULD: Test file names SHOULD mirror the source file being tested (e.g., ComponentName.test.tsx for ComponentName.tsx).
- **R-TEST-005** SHOULD: Test utilities for common setup patterns (store configuration, SSR environment setup) SHOULD be established and reused.
- **R-TEST-006** MAY: Temporary test skipping using .skip MAY be used only with a linked issue and must be resolved within one sprint.

### Verify

```bash
# Count test files with correct extensions
find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l

# Verify React Testing Library usage in component tests
grep -r "import.*@testing-library/react" tests/ || grep -r "from '@testing-library/react'" tests/

# Verify test configuration file exists
test -f vitest.config.ts || test -f jest.config.ts || test -f jest.config.js
```

**Accept when:**
- All test files in the tests directory use .test.ts or .test.tsx extensions
- Test configuration file (vitest.config.ts or jest.config.ts) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (.test.tsx)
- CI/CD pipeline runs all tests and passes without failures
- Code coverage reports are generated and reviewed

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All test files must conform to the TypeScript and React Testing Library standards before code is accepted.
</enforcement>