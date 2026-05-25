# Adopt Core Library Pattern for State Management Middleware: Server Side Rendering

These rules are ALWAYS ACTIVE for all state management middleware modules, vanilla JavaScript state management implementations, React-specific state management hooks and components, example applications demonstrating library usage, and build and distribution configuration for the library.

### Rules

- **R-SSR-001** MUST: Server-side rendering scenarios MUST use the ssrSafe middleware to prevent hydration mismatches.
- **R-SSR-002** MUST: All middleware implementations MUST be placed in src/middleware/ with consistent naming conventions (e.g., redux.ts, immer.ts, persist.ts).
- **R-SSR-003** MUST: Each middleware module MUST export a factory function that returns the middleware, enabling configuration at composition time.
- **R-SSR-004** MUST: Core middleware files (redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts) MUST be present and export middleware factory functions.
- **R-SSR-005** MUST: Separate test suites for vanilla (tests/vanilla/) and React-specific (tests/) implementations MUST be maintained to ensure both targets work correctly.
- **R-SSR-006** MUST: Build tooling (rollup.config.mjs) MUST be configured to produce separate bundles for each middleware module to enable tree-shaking.
- **R-SSR-007** SHOULD: Example applications in examples/ directory SHOULD demonstrate common usage patterns and middleware combinations.
- **R-SSR-008** SHOULD: Middleware composition order SHOULD be clearly documented with examples showing correct ordering and runtime warnings for common mistakes.

### Verify

```bash
# Verify at least 5 middleware modules exist in src/middleware/
find src/middleware -name '*.ts' | wc -l | grep -E '[5-9]|[1-9][0-9]+'

# Verify middleware factory exports
grep -r 'export.*middleware' src/middleware/ | wc -l | grep -E '[5-9]|[1-9][0-9]+'

# Verify core middleware files are present
test -f src/middleware/redux.ts && test -f src/middleware/immer.ts && test -f src/middleware/persist.ts && test -f src/middleware/combine.ts && test -f src/middleware/ssrSafe.ts && echo 'Core middleware files present'

# Verify test suite passes
npm test 2>&1 | grep -E '(PASS|✓).*test' | wc -l | grep -E '[1-9][0-9]+'
```

**Accept when:**
- At least 5 middleware modules exist in src/middleware/ directory with consistent TypeScript implementation
- Core middleware files (redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts) are present and export middleware factory functions
- Test suite passes with comprehensive coverage of vanilla and React implementations
- Example applications successfully demonstrate middleware composition patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting changes. Violations detected by CI pipeline, code review, or build process MUST result in rejection or remediation.
</enforcement>