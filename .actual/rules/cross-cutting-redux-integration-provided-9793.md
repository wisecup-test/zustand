# Adopt Core Library Pattern for State Management Middleware: Redux Integration Provided

These rules are ALWAYS ACTIVE for all state management middleware modules, vanilla JavaScript implementations, React-specific hooks and components, example applications, and build configuration files within the project's state management library.

### Rules

- **R-REDUX-001** SHOULD: Redux integration SHOULD be provided through the redux middleware for applications requiring Redux DevTools or Redux ecosystem compatibility.
- **R-REDUX-002** MUST: All middleware implementations MUST be placed in src/middleware/ with consistent naming conventions (e.g., redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts).
- **R-REDUX-003** MUST: Each middleware module MUST export a factory function that returns the middleware, enabling configuration at composition time.
- **R-REDUX-004** MUST: Separate test suites MUST be maintained for vanilla (tests/vanilla/) and React-specific (tests/) implementations to ensure both targets work correctly.
- **R-REDUX-005** SHOULD: Example applications SHOULD be provided in examples/ directory demonstrating common usage patterns and middleware combinations.
- **R-REDUX-006** MUST: Build tooling (rollup.config.mjs) MUST be configured to produce separate bundles for each middleware module to enable tree-shaking.
- **R-REDUX-007** SHOULD: Clear documentation and examples SHOULD be provided showing correct middleware ordering, with runtime warnings for common ordering mistakes.
- **R-REDUX-008** SHOULD: Performance benchmarks SHOULD be implemented in the test suite to detect middleware overhead in applications with frequent state updates.

### Verify

```bash
# Verify at least 5 middleware modules exist in src/middleware/
find src/middleware -name '*.ts' | wc -l | grep -E '[5-9]|[1-9][0-9]+'

# Verify middleware factory exports are present
grep -r 'export.*middleware' src/middleware/ | wc -l | grep -E '[5-9]|[1-9][0-9]+'

# Verify core middleware files are present
test -f src/middleware/redux.ts && test -f src/middleware/immer.ts && test -f src/middleware/persist.ts && test -f src/middleware/combine.ts && test -f src/middleware/ssrSafe.ts && echo 'Core middleware files present'

# Verify test suite passes with comprehensive coverage
npm test 2>&1 | grep -E '(PASS|✓).*test' | wc -l | grep -E '[1-9][0-9]+'
```

**Accept when:**
- At least 5 middleware modules exist in src/middleware/ directory with consistent TypeScript implementation
- Core middleware files (redux.ts, immer.ts, persist.ts, combine.ts, ssrSafe.ts) are present and export middleware factory functions
- Test suite passes with comprehensive coverage of vanilla and React implementations
- Example applications successfully demonstrate middleware composition patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All verify commands MUST pass before accepting changes to state management middleware. CI pipeline failures or test suite failures MUST block deployment.
</enforcement>