# Adopt Modular Middleware Pattern for Public API Extension: Middleware Composable Allowing

These rules are ALWAYS ACTIVE for all public API extension points, middleware modules distributed as part of the official library package, third-party middleware following the established middleware contract, and framework-specific integrations implemented as middleware.

### Rules

- **R-MW-001** SHOULD: Middleware SHOULD be composable, allowing multiple middleware to be chained without conflicts.

### Verify

```bash
# Verify middleware exports are consistent
grep -r "export.*middleware" src/middleware/ | wc -l

# Count middleware modules
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

<enforcement>
Claude Code MUST NOT skip or defer verification. Type errors in middleware implementations MUST fail the CI build. Breaking changes to middleware API MUST trigger major version bump requirement. Code review MUST reject PRs that violate middleware composition patterns. Bundle size regressions MUST trigger investigation and potential refactoring.
</enforcement>