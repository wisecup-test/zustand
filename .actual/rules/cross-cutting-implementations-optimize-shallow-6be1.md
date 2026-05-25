# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Implementations Optimize Shallow

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-SHALLOW-001** MAY: Implementations MAY optimize shallow comparison by caching property keys or using fast-path checks for common object types.

### Verify

```bash
# Verify shallow comparison usage across codebase
grep -r "shallowEqual\|shallow" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/

# Run shallow comparison and state change detection tests
npm test -- --testNamePattern="shallow.*comparison|state.*change.*detection"

# Verify immutable update patterns with ESLint
eslint src/ --rule 'no-param-reassign: error' --rule 'immutable/no-mutation: error'
```

**Accept when:**
- All datastore implementations use a shared shallow comparison utility or documented equivalent algorithm
- Unit tests verify that shallow comparison correctly detects property-level changes and ignores nested mutations
- Code review checklist includes verification of immutable update patterns in state mutations
- Performance tests demonstrate comparison operations complete within acceptable latency bounds (e.g., <1ms p95)

<enforcement>
Claude Code MUST NOT skip or defer verification. All datastore implementations MUST adhere to shallow comparison patterns for state change detection. Violations block CI pipeline and require explicit tech lead approval with documented justification.
</enforcement>