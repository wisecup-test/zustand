# Adopt Message Queue Pattern for Middleware Communication: Middleware Subscribe Specific

These rules are ALWAYS ACTIVE for all middleware components requiring inter-component communication, including devtools middleware, persistence middleware, test infrastructure, and integration patterns between API layers and middleware.

### Rules

- **R-MQ-001** SHOULD: Middleware SHOULD subscribe to specific message types or channels to receive relevant communications.

### Verify

```bash
# Check for message queue pattern usage in middleware
grep -r "message.*queue\|queue.*message" src/middleware/ --include="*.ts" | wc -l

# Check for direct middleware invocation patterns (should be minimized)
grep -r "direct.*call\|invoke.*middleware" src/middleware/ --include="*.ts" | wc -l

# Run tests for message queue boundaries
npm test -- --grep "message queue" 2>&1 | grep -c "passing"
```

**Accept when:**
- Message queue pattern is detected in middleware communication code with grep showing positive matches
- Direct middleware invocation patterns are minimized or eliminated (grep count approaches zero)
- Tests for message queue boundaries pass successfully, confirming isolation and proper communication

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated code review checks MUST scan for direct middleware-to-middleware method calls. CI pipeline verification commands MUST check for message queue pattern usage. Architecture review MUST occur during pull request process for new middleware components. CI build MUST fail if direct middleware coupling is detected without documented exception.
</enforcement>