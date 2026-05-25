# Adopt Modular Middleware Pattern for Public API Extension: Middleware Expose Additional

These rules are ALWAYS ACTIVE for all public API extension points, middleware modules distributed as part of the official library package, third-party middleware following the established middleware contract, and framework-specific integrations implemented as middleware.

### Rules

- **R-MW-001** MAY: Middleware MAY expose additional utility functions or helpers specific to their domain (e.g., shallow comparison utilities).
- **R-MW-002** MUST: All middleware modules export functions with consistent type signatures that accept configuration and return enhanced API.
- **R-MW-003** MUST: Middleware implementations preserve type information through composition using TypeScript generics.
- **R-MW-004** SHOULD: Middleware should be implemented as separate TypeScript modules with clear type definitions in the src/middleware/ directory.
- **R-MW-005** SHOULD: Provide a combine utility to help consumers compose multiple middleware safely.
- **R-MW-006** SHOULD: Include SSR-safe implementations for middleware that may run in server environments.
- **R-MW-007** MUST: Breaking changes to middleware API trigger major version bump requirement.
- **R-MW-008** MUST: Middleware must remain independently tree-shakeable and not introduce bundle size regressions.

### Verify

```bash
# Count exported middleware functions
grep -r "export.*middleware" src/middleware/ | wc -l

# Count middleware modules with exports
find src/middleware -name '*.ts' -type f | xargs grep -l 'export' | wc -l

# Run type checking and middleware tests
npm run type-check && npm test -- --grep middleware

# Verify bundle size remains acceptable
npm run build && npm run check:bundle-size
```

**Accept when:**
- All middleware modules export functions with consistent type signatures that accept configuration and return enhanced API
- Type checking passes without errors for middleware composition scenarios
- Test suite includes integration tests for common middleware combinations
- Documentation includes middleware authoring guide and API reference for each official middleware
- Middleware remains independently tree-shakeable with no bundle size regressions

<enforcement>
Claude Code MUST NOT skip or defer verification. Type errors in middleware implementations MUST fail CI build. Breaking changes to middleware API MUST trigger major version bump requirement. Code review MUST reject PRs that violate middleware composition patterns. Bundle size regressions MUST trigger investigation and potential refactoring.
</enforcement>