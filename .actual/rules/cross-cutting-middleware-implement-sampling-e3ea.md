# Standardize Middleware Logging for Development and Persistence Operations: Middleware Implement Sampling

These rules are ALWAYS ACTIVE for all middleware components in `src/middleware/` that handle development tooling or state persistence operations, including devtools, hot-reload, debugging, session, cache, database, and request/response transformation middleware.

### Rules

- **R-MIDDLEWARE-001** MAY: Middleware MAY implement sampling or rate-limiting for high-frequency operations to manage log volume.
- **R-MIDDLEWARE-002** MUST: All middleware files in `src/middleware/` contain at least one logging statement per major operation.
- **R-MIDDLEWARE-003** MUST: Devtools.ts and persist.ts middleware include structured logging with request context (request_id, timestamp).
- **R-MIDDLEWARE-004** SHOULD: Create a shared logging utility module for middleware to ensure consistent log format and context extraction.
- **R-MIDDLEWARE-005** SHOULD: Define standard log fields for middleware: timestamp, request_id, middleware_name, operation, duration_ms, status.
- **R-MIDDLEWARE-006** SHOULD: Implement log level configuration through environment variables to enable different verbosity in development vs production.

### Verify

```bash
# Count logging statements in middleware
grep -r "logger\|console\.log\|console\.error" src/middleware/ | wc -l

# Verify structured logging with request context in devtools and persist
grep -r "request_id\|timestamp" src/middleware/*.ts | grep -E "(devtools|persist)" | wc -l

# Run middleware logging tests
npm test -- --grep "middleware.*logging" --reporter json | jq '.tests | length'
```

**Accept when:**
- All middleware files in `src/middleware/` contain at least one logging statement per major operation
- Devtools.ts and persist.ts middleware include structured logging with request context (request_id, timestamp)
- Middleware logging tests pass with coverage >80% for logging code paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Pull requests adding new middleware without logging are blocked by automated checks. Existing middleware without logging is flagged in technical debt backlog for remediation.
</enforcement>