# Adopt Modular Middleware Pattern for Public API Extension: Public Expose Middleware

These rules are ALWAYS ACTIVE for all public API extension points, middleware modules distributed as part of the official library package, third-party middleware following the established middleware contract, and framework-specific integrations implemented as middleware.

### Rules

- **R-MIDDLEWARE-001** MUST: Public APIs MUST expose a middleware composition mechanism that allows consumers to intercept and augment core functionality.
- **R-MIDDLEWARE-002** MUST: All middleware modules MUST export functions with consistent type signatures that accept configuration and return enhanced API.
- **R-MIDDLEWARE-003** MUST: Each middleware MUST be a separate TypeScript module with clear type definitions following the established pattern in src/middleware/ directory.
- **R-MIDDLEWARE-004** MUST: Middleware composition MUST preserve type information through TypeScript generics.
- **R-MIDDLEWARE-005** MUST: Breaking changes to middleware API MUST trigger a major version bump requirement.
- **R-MIDDLEWARE-006** SHOULD: Provide a combine utility to help consumers compose multiple middleware safely.
- **R-MIDDLEWARE-007** SHOULD: Include SSR-safe implementations for middleware that may run in server environments.
- **R-MIDDLEWARE-008** SHOULD: Document middleware signature patterns and provide reference implementations for common use cases (persistence, dev tools, framework integration).
- **R-MIDDLEWARE-009** SHOULD: Enforce semantic versioning and maintain comprehensive test suite covering middleware composition.
- **R-MIDDLEWARE-010** SHOULD: Provide performance guidelines in documentation and include performance tests for official middleware.
- **R-MIDDLEWARE-011** SHOULD: Document composition order semantics clearly and test common middleware combinations.

### Verify

```bash
# Count exported middleware modules
grep -r "export.*middleware" src/middleware/ | wc -l

# Count middleware files with exports
find src/middleware -name '*.ts' -type f | xargs grep -l 'export' | wc -l

# Run type checking and middleware tests
npm run type-check && npm test -- --grep middleware
```

**Accept when:**
- All middleware modules export functions with consistent type signatures that accept configuration and return enhanced API
- Type checking passes without errors for middleware composition scenarios
- Test suite includes integration tests for common middleware combinations
- Documentation includes middleware authoring guide and API reference for each official middleware
- Middleware remains independently tree-shakeable as verified by bundle size checks
- All middleware implementations pass TypeScript compiler type safety checks

<enforcement>
Claude Code MUST NOT skip or defer verification. Type errors in middleware implementations MUST fail CI build. Breaking changes to middleware API MUST trigger major version bump requirement. Code review MUST reject PRs that violate middleware composition patterns. Bundle size regressions MUST trigger investigation and potential refactoring.
</enforcement>