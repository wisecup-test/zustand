# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Shallow Comparison Implementations

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-SHALLOW-001** MUST: Shallow comparison implementations MUST check reference equality first before iterating over object properties for performance optimization.

### Verify

```bash
# Search for shallow comparison implementations
grep -r "shallowEqual\|shallow" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/

# Run tests for shallow comparison and state change detection
npm test -- --testNamePattern="shallow.*comparison|state.*change.*detection"

# Verify immutability patterns with ESLint
eslint src/ --rule 'no-param-reassign: error' --rule 'immutable/no-mutation: error'
```

**Accept when:**
- All datastore implementations use a shared shallow comparison utility or documented equivalent algorithm
- Unit tests verify that shallow comparison correctly detects property-level changes and ignores nested mutations
- Code review checklist includes verification of immutable update patterns in state mutations
- Performance tests demonstrate comparison operations complete within acceptable latency bounds (e.g., <1ms p95)

<enforcement>
Verification is mandatory. Claude Code MUST NOT skip or defer verification of shallow comparison implementations, immutable update patterns, and performance characteristics before approving changes to state management systems.
</enforcement>