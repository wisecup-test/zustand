# Adopt Core Library Pattern for State Management Middleware: Immutable State Updates

These rules are ALWAYS ACTIVE for all state management middleware modules, vanilla JavaScript state management implementations, React-specific state management hooks and components, example applications, and build configuration files within the configured scope.

### Rules

- **R-IMMUTABLE-001** MUST: Immutable state updates MUST be handled through the Immer middleware when complex nested state modifications are required.

### Verify

```bash
# Verify at least 5 middleware modules exist in src/middleware/
find src/middleware -name '*.ts' | wc -l | grep -E '[5-9]|[1-9][0-9]+'

# Verify middleware factory functions are exported
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
Claude Code MUST NOT skip or defer verification. All verify commands MUST pass before accepting changes to state management middleware.
</enforcement>