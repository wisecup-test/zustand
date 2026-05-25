# Adopt Message Queue Pattern for Middleware Communication: Devtools Persistence Middleware

These rules are ALWAYS ACTIVE for all middleware components requiring inter-component communication, devtools middleware for debugging and state inspection, persistence middleware for state synchronization, test infrastructure requiring middleware interaction verification, and integration patterns between API layers and middleware.

### Rules

- **R-MQ-001** SHOULD: Devtools and persistence middleware SHOULD use message queues for state synchronization and debugging communication.
- **R-MQ-002** MUST: Avoid direct method calls between middleware components; use message queue boundaries instead.
- **R-MQ-003** SHOULD: Implement message queue abstraction layer to allow different queue implementations (in-memory for tests, production queue for deployment).
- **R-MQ-004** SHOULD: Add correlation IDs to all messages for tracing and debugging purposes.
- **R-MQ-005** SHOULD: Create clear message schemas and documentation for each message type used in middleware communication.
- **R-MQ-006** SHOULD: Implement monitoring and alerting for queue depth, message processing latency, and failure rates.
- **R-MQ-007** MAY: Performance-critical paths where message queue overhead is demonstrably unacceptable (>10ms latency impact) may be exempted with documented exception (EXC-001).
- **R-MQ-008** MAY: Legacy middleware components during migration period may be exempted with documented exception (EXC-002).

### Verify

```bash
# Count message queue pattern usage in middleware
grep -r "message.*queue\|queue.*message" src/middleware/ --include="*.ts" | wc -l

# Count direct middleware invocation patterns (should be minimized)
grep -r "direct.*call\|invoke.*middleware" src/middleware/ --include="*.ts" | wc -l

# Run tests for message queue boundaries
npm test -- --grep "message queue" 2>&1 | grep -c "passing"
```

**Accept when:**
- Message queue pattern is detected in middleware communication code with grep showing positive matches
- Direct middleware invocation patterns are minimized or eliminated (grep count approaches zero)
- Tests for message queue boundaries pass successfully, confirming isolation and proper communication
- All new middleware components implement message queue pattern for inter-component communication
- Correlation IDs are present in message implementations for tracing

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated code review checks MUST scan for direct middleware-to-middleware method calls. CI pipeline verification commands MUST check for message queue pattern usage. Architecture review MUST occur during pull request process for new middleware components. CI build MUST fail if direct middleware coupling is detected without documented exception. Pull requests MUST be blocked until message queue pattern is properly implemented.
</enforcement>