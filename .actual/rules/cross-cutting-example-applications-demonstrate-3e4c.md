# Adopt Core Library Pattern for State Management Middleware: Example Applications Demonstrate

These rules are ALWAYS ACTIVE for all state management middleware modules, example applications, test suites, and build configuration files within the project's scope.

### Rules

- **R-SM-001** SHOULD: Example applications SHOULD demonstrate practical usage patterns for both simple and complex state management scenarios.
- **R-SM-002** MUST: All middleware implementations MUST be placed in `src/middleware/` with consistent naming conventions (e.g., redux.ts, immer.ts, persist.ts).
- **R-SM-003** MUST: Each middleware module MUST export a factory function that returns the middleware, enabling configuration at composition time.
- **R-SM-004** MUST: Separate test suites MUST be maintained for vanilla (tests/vanilla/) and React-specific (tests/) implementations to ensure both targets work correctly.
- **R-SM-005** SHOULD: Example applications SHOULD be provided in examples/ directory demonstrating common usage patterns and middleware combinations.
- **R-SM-006** MUST: Build tooling (rollup.config.mjs) MUST be configured to produce separate bundles for each middleware module to enable tree-shaking.
- **R-SM-007** MUST: Core middleware files (redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts) MUST be present and export middleware factory functions.
- **R-SM-008** MUST: Test suite MUST pass with comprehensive coverage of vanilla and React implementations.
- **R-SM-009** SHOULD: Documentation SHOULD provide clear examples showing correct middleware ordering and implement runtime warnings for common ordering mistakes.
- **R-SM-010** SHOULD: Performance benchmarks SHOULD be implemented in test suite with guidance on when to use lightweight alternatives.

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
- At least 5 middleware modules exist in `src/middleware/` directory with consistent TypeScript implementation
- Core middleware files (redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts) are present and export middleware factory functions
- Test suite passes with comprehensive coverage of vanilla and React implementations
- Example applications successfully demonstrate middleware composition patterns
- Build process validates that middleware modules are properly exported and tree-shakeable

<enforcement>
Claude Code MUST NOT skip or defer verification. CI pipeline MUST run test suite on every pull request. Code review process MUST verify new middleware follows established patterns. Build process MUST validate middleware module exports and tree-shakeability. Pull requests introducing non-composable middleware patterns MUST be rejected. Build failures MUST prevent deployment of improperly structured middleware modules.
</enforcement>