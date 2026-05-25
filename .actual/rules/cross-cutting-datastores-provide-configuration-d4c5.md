# Adopt Shallow Comparison for State Change Detection in Primary Datastores: Datastores Provide Configuration

These rules are ALWAYS ACTIVE for all state management and datastore implementations that require change detection and subscription notification mechanisms.

### Rules

- **R-DATASTORE-001** SHOULD: Datastores SHOULD provide configuration options to customize comparison behavior for specific use cases requiring deep equality.

### Verify

```bash
# Verify shallow comparison utility exists and is used
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
Verified by: Automated unit tests for shallow comparison behavior in all datastore modules
Verified by: Code review checklist item requiring verification of immutable state patterns
Verified by: Static analysis with ESLint rules enforcing immutability (no-param-reassign, immutable/no-mutation)
Verified by: Performance regression tests monitoring comparison operation latency
Violation handling: CI pipeline fails if shallow comparison tests are missing or failing
Violation handling: ESLint violations for mutation patterns block PR merge
Violation handling: Code review requires explicit justification and tech lead approval for any deep comparison usage
Violation handling: Performance regression alerts trigger investigation and potential rollback
Exception process: Developer documents exception request with performance data or technical justification
Exception process: Tech lead reviews and approves/rejects based on documented criteria (EXC-001, EXC-002)
Exception process: Approved exceptions are tracked in ADR amendments or inline code documentation
Exception process: Exceptions are reviewed quarterly for potential remediation or pattern updates
</enforcement>