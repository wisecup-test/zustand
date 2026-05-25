# Standardize Middleware Logging for Development and Persistence Operations: Error Conditions Middleware

These rules are ALWAYS ACTIVE for all middleware components in `src/middleware/` that handle development tooling or state persistence operations, including devtools, hot-reload, debugging, session, cache, database, and request/response transformation middleware.

### Rules

- **R-MIDDLEWARE-001** MUST: Error conditions in middleware MUST be logged with sufficient detail to enable root cause analysis.

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

<enforcement>
Claude Code MUST NOT skip or defer verification. All middleware components must include error condition logging before pull requests are approved.
</enforcement>