# Adopt Vitest/Jest with TypeScript and React Testing Library as Standard Testing Framework: Tests Cover Specialized

These rules are ALWAYS ACTIVE for all test files in the `tests/` directory and any TypeScript/React component test files using `.test.ts` or `.test.tsx` extensions.

### Rules

- **R-TEST-001** SHOULD: Tests SHOULD cover specialized React patterns including state management libraries (Immer), shallow comparison, and server-side rendering scenarios.
- **R-TEST-002** MUST: All test files MUST use `.test.ts` or `.test.tsx` extensions.
- **R-TEST-003** MUST: Test configuration file (vitest.config.ts or jest.config.ts) MUST exist and include TypeScript support.
- **R-TEST-004** SHOULD: React Testing Library SHOULD be imported and used in component test files (`.test.tsx`).
- **R-TEST-005** SHOULD: Test file names SHOULD mirror the source file being tested (e.g., `ComponentName.test.tsx` for `ComponentName.tsx`).
- **R-TEST-006** MUST: All unit tests for React components, vanilla JavaScript/TypeScript modules, and integration tests involving state management libraries MUST be included in scope.
- **R-TEST-007** MUST: End-to-end (E2E) tests, performance benchmarking tests, visual regression tests, and load/stress testing MUST be out of scope for this rule.

### Verify

```bash
# Count test files with correct extensions
find tests -type f \( -name '*.test.ts' -o -name '*.test.tsx' \) | wc -l

# Check for React Testing Library imports in component tests
grep -r "import.*@testing-library/react" tests/ || grep -r "from '@testing-library/react'" tests/

# Verify test configuration file exists
test -f vitest.config.ts || test -f jest.config.ts || test -f jest.config.js
```

**Accept when:**
- All test files in the `tests/` directory use `.test.ts` or `.test.tsx` extensions
- Test configuration file (`vitest.config.ts` or `jest.config.ts`) exists and includes TypeScript support
- React Testing Library is imported and used in component test files (`.test.tsx`)
- Test files follow naming conventions that mirror source files being tested
- Specialized React patterns (Immer, SSR, shallow comparison) are covered by dedicated test files

<enforcement>
Claude Code MUST NOT skip or defer verification. All verification commands MUST pass before accepting changes to test files or test configuration.
</enforcement>