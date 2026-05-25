# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Primary Datastores Implement

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-SHALLOW-001** MUST: Primary datastores MUST implement shallow comparison for state change detection when determining whether to notify subscribers or update caches.

### Verify

```bash
# Search for shallow comparison implementations
grep -r "shallowEqual\|shallow" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/

# Run tests for shallow comparison and state change detection
npm test -- --testNamePattern="shallow.*comparison|state.*change.*detection"

# Lint for mutation patterns that violate immutability
eslint src/ --rule 'no-param-reassign: error' --rule 'immutable/no-mutation: error'
```

**Accept when:**
- All datastore implementations use a shared shallow comparison utility or documented equivalent algorithm
- Unit tests verify that shallow comparison correctly detects property-level changes and ignores nested mutations
- Code review checklist includes verification of immutable update patterns in state mutations
- Performance tests demonstrate comparison operations complete within acceptable latency bounds (e.g., <1ms p95)

<enforcement>
Claude Code MUST NOT skip or defer verification. All datastore implementations MUST be audited for shallow comparison usage. ESLint rules enforcing immutability MUST pass. Performance regression tests MUST confirm acceptable latency bounds. CI pipeline MUST fail if shallow comparison tests are missing or failing.
</enforcement>