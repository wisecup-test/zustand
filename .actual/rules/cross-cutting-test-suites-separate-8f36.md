# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Test Suites Separate

These rules are ALWAYS ACTIVE for all unit tests, integration tests, and test configuration files in the project.

### Rules

- **R-TEST-001** SHOULD: Test suites SHOULD separate vanilla JavaScript logic tests from React component tests using directory structure.
- **R-TEST-002** MUST: All test files MUST use `.test.ts` or `.test.tsx` extensions.
- **R-TEST-003** MUST: Test configuration file (vitest.config.ts or jest.config.ts) MUST exist and include TypeScript support.
- **R-TEST-004** MUST: React component test files (`.test.tsx`) MUST import and use React Testing Library.
- **R-TEST-005** SHOULD: Test file names SHOULD mirror the source file being tested (e.g., `ComponentName.test.tsx` for `ComponentName.tsx`).
- **R-TEST-006** MUST: All new features and bug fixes MUST include corresponding test files.
- **R-TEST-007** SHOULD: Code coverage reports SHOULD be generated and reviewed; significant coverage drops SHOULD block merges.
- **R-TEST-008** MAY: Temporary test skipping (using `.skip`) MAY be used only with a linked issue and must be resolved within one sprint.

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
- All test files in the tests directory use `.test.ts` or `.test.tsx` extensions
- Test configuration file (vitest.config.ts or jest.config.ts) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (`.test.tsx`)
- Test files follow naming conventions that mirror source files
- New features and bug fixes include corresponding test coverage

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All test files must conform to the naming and organization standards defined herein. Code review and CI/CD pipeline enforcement is mandatory.
</enforcement>