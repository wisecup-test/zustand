# Adopt Modular Middleware Pattern for Public API Extension: Core Library Provide

These rules are ALWAYS ACTIVE for all public API extension points, middleware modules distributed as part of the official library package, third-party middleware following the established middleware contract, and framework-specific integrations implemented as middleware.

### Rules

- **R-MIDDLEWARE-001** SHOULD: Core library SHOULD provide reference middleware implementations for common use cases (persistence, dev tools, framework integration).
- **R-MIDDLEWARE-002** MUST: Each middleware module MUST be a separate TypeScript module with clear type definitions in the src/middleware/ directory.
- **R-MIDDLEWARE-003** MUST: All middleware modules MUST export functions with consistent type signatures that accept configuration and return enhanced API.
- **R-MIDDLEWARE-004** MUST: Middleware implementations MUST use TypeScript generics to preserve type information through middleware composition.
- **R-MIDDLEWARE-005** SHOULD: Middleware composition SHOULD be facilitated through a combine utility (src/middleware/combine.ts) to help consumers compose multiple middleware safely.
- **R-MIDDLEWARE-006** SHOULD: Middleware implementations SHOULD include SSR-safe variants (src/middleware/ssrSafe.ts) for middleware that may run in server environments.
- **R-MIDDLEWARE-007** MUST: Breaking changes to middleware API MUST trigger a major version bump requirement.
- **R-MIDDLEWARE-008** MUST: Middleware implementations MUST remain independently tree-shakeable and not increase bundle size for consumers who don't use them.
- **R-MIDDLEWARE-009** SHOULD: Middleware composition order semantics SHOULD be clearly documented to prevent unexpected behavior or conflicts.
- **R-MIDDLEWARE-010** MUST: Type checking MUST pass without errors for all middleware composition scenarios.

### Verify

```bash
# Count exported middleware modules
grep -r "export.*middleware" src/middleware/ | wc -l

# Count middleware files with exports
find src/middleware -name '*.ts' -type f | xargs grep -l 'export' | wc -l

# Run type checking and middleware tests
npm run type-check && npm test -- --grep middleware

# Verify bundle size for individual middleware
npm run build && npm run analyze -- --filter middleware
```

**Accept when:**
- All middleware modules export functions with consistent type signatures that accept configuration and return enhanced API
- Type checking passes without errors for middleware composition scenarios
- Test suite includes integration tests for common middleware combinations
- Documentation includes middleware authoring guide and API reference for each official middleware
- Middleware remains independently tree-shakeable with no bundle size regressions
- All middleware implementations follow the established pattern in src/middleware/ directory

<enforcement>
Claude Code MUST NOT skip or defer verification. Type errors in middleware implementations MUST fail the CI build. Breaking changes to middleware API MUST trigger major version bump requirements. Code review MUST reject PRs that violate middleware composition patterns. Bundle size regressions MUST trigger investigation and potential refactoring.
</enforcement>