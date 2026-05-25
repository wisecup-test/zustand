# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: React Component Tests

These rules are ALWAYS ACTIVE for all React component test files and TypeScript/JavaScript test files in the project's test suite.

### Rules

- **R-REACT-TEST-001** MUST: React component tests MUST use the .test.tsx extension to support JSX syntax.
- **R-REACT-TEST-002** MUST: All test files in the tests directory MUST use .test.ts or .test.tsx extensions.
- **R-REACT-TEST-003** MUST: Test configuration file (vitest.config.ts or jest.config.ts) MUST exist and include TypeScript support.
- **R-REACT-TEST-004** MUST: React Testing Library MUST be imported and used in component test files (.test.tsx).
- **R-REACT-TEST-005** SHOULD: Test file names SHOULD mirror the source file being tested (e.g., ComponentName.test.tsx for ComponentName.tsx).
- **R-REACT-TEST-006** SHOULD: Test utilities for common setup patterns (store configuration, SSR environment setup) SHOULD be established and maintained.
- **R-REACT-TEST-007** MAY: Temporary test skipping (using .skip) MAY be used only with a linked issue and must be resolved within one sprint.

### Verify

```bash
# Count test files with proper extensions
find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l

# Verify React Testing Library is imported in component tests
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
Claude Code MUST NOT skip or defer verification of these rules. All test files must conform to the naming conventions and framework requirements specified. Pull requests without tests for new features or bug fixes MUST be rejected. Test files not following naming conventions MUST trigger CI warnings. Code coverage drops MUST block merges unless explicitly approved through the exception process.
</enforcement>