# Adopt Shallow Comparison for State Change Detection in Primary Datastores: State Objects Treated

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-STATE-001** MUST: State objects MUST be treated as immutable, with mutations creating new object references to ensure shallow comparison correctness.

### Verify

```bash
# Verify shallow comparison utilities are in use
grep -r "shallowEqual\|shallow" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/

# Run shallow comparison and state change detection tests
npm test -- --testNamePattern="shallow.*comparison|state.*change.*detection"

# Verify immutability enforcement via ESLint
eslint src/ --rule 'no-param-reassign: error' --rule 'immutable/no-mutation: error'
```

**Accept when:**
- All datastore implementations use a shared shallow comparison utility or documented equivalent algorithm
- Unit tests verify that shallow comparison correctly detects property-level changes and ignores nested mutations
- Code review checklist includes verification of immutable update patterns in state mutations
- Performance tests demonstrate comparison operations complete within acceptable latency bounds (e.g., <1ms p95)
- ESLint rules enforcing immutability (no-param-reassign, immutable/no-mutation) pass without violations

<enforcement>
Claude Code MUST NOT skip or defer verification. All datastore implementations MUST adhere to immutable state patterns and use shallow comparison for change detection. Violations block PR merge and trigger CI pipeline failures.
</enforcement>