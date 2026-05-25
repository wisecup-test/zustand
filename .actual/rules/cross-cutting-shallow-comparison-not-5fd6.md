# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Shallow Comparison Not

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-SHALLOW-001** MUST_NOT: Shallow comparison MUST NOT be used as the sole mechanism when nested object mutations need to be detected without reference changes.

### Verify

```bash
# Verify shallow comparison usage patterns
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
- No in-place mutations of nested state objects are detected in primary datastore implementations
- ESLint immutability rules pass without violations in state management code

<enforcement>
Claude Code MUST NOT skip or defer verification of shallow comparison rules. All datastore implementations MUST be audited for compliance with R-SHALLOW-001. Violations block PR merge until remediated or an approved exception (EXC-001 or EXC-002) is documented.
</enforcement>