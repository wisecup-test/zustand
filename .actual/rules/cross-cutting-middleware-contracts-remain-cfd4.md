# Adopt Modular Middleware Pattern for Public API Extension: Middleware Contracts Remain

These rules are ALWAYS ACTIVE for all public API extension points, middleware modules distributed as part of the official library package, third-party middleware following the established middleware contract, and framework-specific integrations implemented as middleware.

### Rules

- **R-MID-001** MUST: Middleware API contracts MUST remain stable across minor versions to prevent breaking consumer code.

### Verify

```bash
# Verify middleware exports are consistent
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
- Middleware remains independently tree-shakeable as verified by bundle size checks

<enforcement>
Claude Code MUST NOT skip or defer verification. Type errors in middleware implementations MUST fail the CI build. Breaking changes to middleware API MUST trigger a major version bump requirement. Code review MUST reject PRs that violate middleware composition patterns.
</enforcement>