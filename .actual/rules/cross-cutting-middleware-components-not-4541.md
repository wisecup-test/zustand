# Adopt Message Queue Pattern for Middleware Communication: Middleware Components Not

These rules are ALWAYS ACTIVE for all middleware components requiring inter-component communication, including devtools middleware, persistence middleware, test infrastructure, and integration patterns between API layers and middleware.

### Rules

- **R-MQ-001** MUST_NOT: Middleware components MUST NOT maintain direct references to other middleware instances for communication purposes.

### Verify

```bash
# Check for message queue pattern usage in middleware
grep -r "message.*queue\|queue.*message" src/middleware/ --include="*.ts" | wc -l

# Check for direct middleware invocation patterns
grep -r "direct.*call\|invoke.*middleware" src/middleware/ --include="*.ts" | wc -l

# Run tests for message queue boundaries
npm test -- --grep "message queue" 2>&1 | grep -c "passing"
```

**Accept when:**
- Message queue pattern is detected in middleware communication code with grep showing positive matches
- Direct middleware invocation patterns are minimized or eliminated (grep count approaches zero)
- Tests for message queue boundaries pass successfully, confirming isolation and proper communication

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations of R-MQ-001 MUST be caught during code review and CI pipeline checks. Direct middleware coupling without documented exception (EXC-001 or EXC-002) MUST cause CI build failure and pull request blocking.
</enforcement>