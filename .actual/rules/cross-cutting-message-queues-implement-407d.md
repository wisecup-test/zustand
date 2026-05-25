# Adopt Message Queue Pattern for Middleware Communication: Message Queues Implement

These rules are ALWAYS ACTIVE for all middleware components requiring inter-component communication, devtools middleware, persistence middleware, test infrastructure, and integration patterns between API layers and middleware.

### Rules

- **R-MQ-001** MAY: Message queues MAY implement priority handling or ordering guarantees based on middleware requirements.

### Verify

```bash
# Detect message queue pattern usage in middleware communication
grep -r "message.*queue\|queue.*message" src/middleware/ --include="*.ts" | wc -l

# Verify direct middleware invocation patterns are minimized
grep -r "direct.*call\|invoke.*middleware" src/middleware/ --include="*.ts" | wc -l

# Confirm message queue boundary tests pass
npm test -- --grep "message queue" 2>&1 | grep -c "passing"
```

**Accept when:**
- Message queue pattern is detected in middleware communication code with grep showing positive matches
- Direct middleware invocation patterns are minimized or eliminated (grep count approaches zero)
- Tests for message queue boundaries pass successfully, confirming isolation and proper communication

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated code review checks MUST scan for direct middleware-to-middleware method calls. CI pipeline verification commands MUST check for message queue pattern usage. Architecture review MUST occur during pull request process for new middleware components. CI build MUST fail if direct middleware coupling is detected without documented exception. Pull requests MUST be blocked until message queue pattern is properly implemented. Quarterly architecture audits MUST identify and prioritize remediation of violations.
</enforcement>