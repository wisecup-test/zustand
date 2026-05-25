# Standardize Middleware Logging for Development and Persistence Operations: Middleware Components Implement

These rules are ALWAYS ACTIVE for all middleware components in `src/middleware/` that handle development tooling or state persistence operations, including devtools, hot-reload, debugging, session, cache, database, and request/response transformation middleware.

### Rules

- **R-MIDDLEWARE-001** MUST: All middleware components MUST implement structured logging for request entry and exit points.
- **R-MIDDLEWARE-002** MUST: Middleware logging MUST include standard log fields: timestamp, request_id, middleware_name, operation, duration_ms, status.
- **R-MIDDLEWARE-003** MUST: Log level configuration MUST be controllable through environment variables to enable different verbosity in development vs production.
- **R-MIDDLEWARE-004** MUST: All middleware logging statements MUST be reviewed for sensitive data (credentials, PII) and sanitized appropriately.
- **R-MIDDLEWARE-005** SHOULD: Create and use a shared logging utility module for middleware to ensure consistent log format and context extraction.

### Verify

```bash
# Count logging statements in middleware directory
grep -r "logger\|console\.log\|console\.error" src/middleware/ | wc -l

# Verify structured logging with request context in devtools and persist
grep -r "request_id\|timestamp" src/middleware/*.ts | grep -E "(devtools|persist)" | wc -l

# Run middleware logging tests
npm test -- --grep "middleware.*logging" --reporter json | jq '.tests | length'
```

**Accept when:**
- All middleware files in `src/middleware/` contain at least one logging statement per major operation
- `devtools.ts` and `persist.ts` middleware include structured logging with request context (request_id, timestamp)
- Middleware logging tests pass with coverage >80% for logging code paths
- Standard log fields (timestamp, request_id, middleware_name, operation, duration_ms, status) are present in logging output
- Log level configuration is accessible via environment variables

<enforcement>
Claude Code MUST NOT skip or defer verification. All middleware components MUST be audited for compliance with R-MIDDLEWARE-001 through R-MIDDLEWARE-005 before code review approval.
</enforcement>