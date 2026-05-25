# Adopt Message Queue Pattern for Middleware Communication: Message Queue Implementations

These rules are ALWAYS ACTIVE for all middleware components requiring inter-component communication, devtools middleware, persistence middleware, test infrastructure, and integration patterns between API layers and middleware.

### Rules

- **R-MQ-001** MUST: Message queue implementations MUST support asynchronous message passing between middleware layers.

### Verify

```bash
# Check for message queue pattern usage in middleware
grep -r "message.*queue\|queue.*message" src/middleware/ --include="*.ts" | wc -l

# Check for direct middleware invocation patterns (should be minimized)
grep -r "direct.*call\|invoke.*middleware" src/middleware/ --include="*.ts" | wc -l

# Verify message queue tests pass
npm test -- --grep "message queue" 2>&1 | grep -c "passing"
```

**Accept when:**
- Message queue pattern is detected in middleware communication code with grep showing positive matches
- Direct middleware invocation patterns are minimized or eliminated (grep count approaches zero)
- Tests for message queue boundaries pass successfully, confirming isolation and proper communication

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated code review checks MUST scan for direct middleware-to-middleware method calls. CI pipeline verification commands MUST check for message queue pattern usage. Architecture review MUST occur during pull request process for new middleware components. CI build MUST fail if direct middleware coupling is detected without documented exception. Pull requests MUST be blocked until message queue pattern is properly implemented.
</enforcement>