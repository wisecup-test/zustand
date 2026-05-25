# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Shallow Comparison Utilities

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-SHALLOW-001** SHOULD: Shallow comparison utilities SHOULD be extracted into reusable functions to ensure consistent behavior across different datastore implementations.

### Verify

```bash
# Search for shallow comparison patterns across the codebase
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
Verification of shallow comparison utility extraction and immutable state patterns is mandatory. CI pipeline MUST fail if shallow comparison tests are missing or failing, ESLint violations for mutation patterns block PR merge, and performance regression tests MUST monitor comparison operation latency.
</enforcement>