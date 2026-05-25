# Adopt Modular Middleware Pattern for Public API Extension: Middleware Follow Consistent

These rules are ALWAYS ACTIVE for all public API extension points, middleware modules distributed as part of the official library package, third-party middleware following the established middleware contract, and framework-specific integrations implemented as middleware.

### Rules

- **R-MW-001** SHOULD: Middleware SHOULD follow a consistent signature pattern that accepts configuration and returns an enhanced API.

### Verify

```bash
# Verify middleware exports follow consistent patterns
grep -r "export.*middleware" src/middleware/ | wc -l

# Count middleware modules with exports
find src/middleware -name '*.ts' -type f | xargs grep -l 'export' | wc -l

# Run type checking and middleware tests
npm run type-check && npm test -- --grep middleware
```

**Accept when:**
- All middleware modules export functions with consistent type signatures that accept configuration and return enhanced API
- Type checking passes without errors for middleware composition scenarios
- Test suite includes integration tests for common middleware combinations
- Documentation includes middleware authoring guide and API reference for each official middleware

<enforcement>
Claude Code MUST NOT skip or defer verification. Type errors in middleware implementations fail CI build. Breaking changes to middleware API trigger major version bump requirement. Code review rejects PRs that violate middleware composition patterns. Bundle size regressions trigger investigation and potential refactoring.
</enforcement>