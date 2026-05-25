# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Test Files Organized

These rules are ALWAYS ACTIVE for all test files in the project's `tests/` directory and any TypeScript/React component test files using `.test.ts` or `.test.tsx` extensions.

### Rules

- **R-TEST-001** MUST: Test files MUST be organized in a dedicated `tests/` directory with subdirectories for different test contexts (e.g., `vanilla/`, `integration/`, `ssr/`).
- **R-TEST-002** MUST: All test files MUST use `.test.ts` or `.test.tsx` extensions.
- **R-TEST-003** MUST: Test configuration file (`vitest.config.ts`, `jest.config.ts`, or `jest.config.js`) MUST exist and include TypeScript support.
- **R-TEST-004** MUST: React component test files (`.test.tsx`) MUST import and use React Testing Library.
- **R-TEST-005** SHOULD: Test file names SHOULD mirror the source file being tested (e.g., `ComponentName.test.tsx` for `ComponentName.tsx`).
- **R-TEST-006** SHOULD: Test utilities for common setup patterns (store configuration, SSR environment setup) SHOULD be centralized in a shared test utilities library.

### Verify

```bash
# Count test files with correct extensions
find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l

# Verify React Testing Library usage in component tests
grep -r "import.*@testing-library/react" tests/ || grep -r "from '@testing-library/react'" tests/

# Verify test configuration file exists
test -f vitest.config.ts || test -f jest.config.ts || test -f jest.config.js

# Verify no test files exist outside tests directory
find . -path ./tests -prune -o -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) -print | wc -l
```

**Accept when:**
- All test files in the `tests/` directory use `.test.ts` or `.test.tsx` extensions
- Test configuration file (`vitest.config.ts` or `jest.config.ts`) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (`.test.tsx`)
- No test files exist outside the `tests/` directory (except configuration files)
- Test file names follow the pattern of mirroring source file names

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All test files must conform to the organized structure and naming conventions before code review approval.
</enforcement>