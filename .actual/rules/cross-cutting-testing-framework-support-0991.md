# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Testing Framework Support

These rules are ALWAYS ACTIVE for all test files in the `tests/` directory and any files using `.test.ts` or `.test.tsx` extensions throughout the codebase.

### Rules

- **R-TEST-001** MUST: The testing framework MUST support TypeScript, React Testing Library, and JSX/TSX syntax without additional transpilation configuration.
- **R-TEST-002** MUST: All test files MUST use `.test.ts` or `.test.tsx` extensions.
- **R-TEST-003** MUST: Test configuration file (vitest.config.ts or jest.config.ts) MUST exist and include TypeScript support.
- **R-TEST-004** MUST: React Testing Library MUST be imported and used in component test files (`.test.tsx`).
- **R-TEST-005** SHOULD: Test file names SHOULD mirror the source file being tested (e.g., `ComponentName.test.tsx` for `ComponentName.tsx`).
- **R-TEST-006** SHOULD: Create test utility files for common setup patterns (e.g., store configuration for Immer tests, SSR environment setup).
- **R-TEST-007** SHOULD: Configure IDE and editor plugins for test runner integration to enable inline test execution and debugging.
- **R-TEST-008** MAY: Exceptions for test coverage may be granted for experimental features or prototypes with explicit documentation.

### Verify

```bash
# Count test files with proper extensions
find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l

# Check for React Testing Library imports in component tests
grep -r "import.*@testing-library/react" tests/ || grep -r "from '@testing-library/react'" tests/

# Verify test configuration file exists with TypeScript support
test -f vitest.config.ts || test -f jest.config.ts || test -f jest.config.js
```

**Accept when:**
- All test files in the tests directory use `.test.ts` or `.test.tsx` extensions
- Test configuration file (vitest.config.ts or jest.config.ts) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (`.test.tsx`)
- CI/CD pipeline runs all tests and fails on test failures or missing test coverage
- Code review process checks for test file presence and proper naming conventions

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files must conform to the naming convention and framework requirements. Pull requests without tests for new features or bug fixes MUST be rejected. Test files not following naming conventions MUST trigger CI warnings. Code coverage reports MUST be generated and reviewed; significant coverage drops MUST block merges.
</enforcement>