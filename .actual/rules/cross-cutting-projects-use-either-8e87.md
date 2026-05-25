# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Projects Use Either

These rules are ALWAYS ACTIVE for all test files in the project's tests directory and any TypeScript/React component test files.

### Rules

- **R-TEST-001** MUST: Use either Vitest or Jest as the underlying test runner for all unit tests.
- **R-TEST-002** MUST: Write all test files using .test.ts or .test.tsx extensions.
- **R-TEST-003** MUST: Configure the test runner with TypeScript support (ts-jest or native TypeScript handling).
- **R-TEST-004** MUST: Use React Testing Library for all React component tests.
- **R-TEST-005** SHOULD: Organize test files in a dedicated tests directory with subdirectories by context (vanilla, react, etc.).
- **R-TEST-006** SHOULD: Name test files to mirror the source file being tested (e.g., ComponentName.test.tsx for ComponentName.tsx).
- **R-TEST-007** MAY: Use either Vitest or Jest as the underlying test runner, provided they meet TypeScript and React requirements.
- **R-TEST-008** MUST: Create test utility files for common setup patterns (store configuration, SSR environment setup).
- **R-TEST-009** MUST NOT: Use plain JavaScript (.test.js) for tests in TypeScript-based projects.
- **R-TEST-010** MUST NOT: Use deprecated testing libraries like Enzyme for new tests.

### Verify

```bash
# Count test files with proper extensions
find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l

# Verify React Testing Library is imported in component tests
grep -r "import.*@testing-library/react" tests/ || grep -r "from '@testing-library/react'" tests/

# Verify test configuration file exists
test -f vitest.config.ts || test -f jest.config.ts || test -f jest.config.js

# Verify no plain JavaScript test files exist
find tests -type f -name '*.test.js' | wc -l
```

**Accept when:**
- All test files in the tests directory use .test.ts or .test.tsx extensions
- Test configuration file (vitest.config.ts or jest.config.ts or jest.config.js) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (.test.tsx)
- No plain .test.js files exist in the tests directory
- Test files are organized by context (vanilla, react, etc.) in subdirectories

<enforcement>
Claude Code MUST NOT skip or defer verification. All test files MUST conform to the Vitest/Jest with TypeScript and React Testing Library standard. Violations block pull request merges.
</enforcement>